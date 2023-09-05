# Install Kolla-Ansible All-in-One (AIO)

## Duration

Approximately 2 hours

## Objective

To understand and install OpenStack using Kolla-Ansible All-in-One (AIO) on Ubuntu 22.04.

## Prerequisites:

1. **Servers**: One server with at least 8GB of RAM and 4 CPU cores. This server will be managed by Kolla-Ansible and
   will run all OpenStack services.
2. **Operating System**: Ubuntu 22.04 LTS.

## Your Environment

You server must have two network interfaces both connected to the same network and same gateway.

- **First interface**: This interface will be used for OpenStack services, It must have an IP address. We are going to
  call it `INTERFACE_NAME` and its IP as `YOUR_IP`.
- **Second interface**: This interface will be used for external network, It must not have an IP address. We are going
  to call it `INTERFACE_NAME_NO_IP`.

**NOTE:** In production environments, you should use two different networks for OpenStack services and external.

You could use the following command to get the name of the interfaces:

```bash
ip a
```

Result would be like:

```bash
pure@pure:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether fa:16:3e:55:4b:bf brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    inet xxx.xxx.xxx.xxx/24 metric 100 brd xxx.xxx.xxx.xxx scope global dynamic ens3
       valid_lft 77405sec preferred_lft 77405sec
    inet6 fe80::f816:3eff:fe55:4bbf/64 scope link
       valid_lft forever preferred_lft forever
4: ens7: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether fa:16:3e:30:2f:32 brd ff:ff:ff:ff:ff:ff
    altname enp0s7
    inet6 fe80::f816:3eff:fe30:2f32/64 scope link
       valid_lft forever preferred_lft forever
```

In this case, `ens3` is the first interface with IP and `ens7` is the second interface without IP.

## Step-by-step guide:

### Install Dependencies

Kolla-Ansible relies on some Python libraries that are not available in the default Ubuntu setup, so you need to install
them.

### Update Repositories

```bash
sudo apt-get update
```

### Install Dependencies

```bash
sudo apt install python3-dev libffi-dev gcc libssl-dev python3-pip git
```

### Install Pip

```bash
sudo pip3 install -U pip
```

### Install Ansible

Kolla Ansible requires at least Ansible 6 and supports up to 7. Install Ansible using pip:

```bash
sudo pip3 install 'ansible>=6,<8'
```

To verify the installation, run:

```bash
ansible --version
```

Result would be like:

```bash
ansible [core 2.14.9]
  config file = None
  configured module search path = ['/home/farshid/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.10/dist-packages/ansible
  ansible collection location = /home/farshid/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

**Note:** Depending on the repositories that you use, `[core 2.14.9]` might be different for you, but since we have
locked the version of ansible, it should be fine.

### Edit Ansible configuration file

```bash
sudo mkdir /etc/ansible
sudo nano /etc/ansible/ansible.cfg
```

Contents:

```bash
[defaults]
host_key_checking=False
pipelining=True
forks=100
```

Let's explain the above configuration:

- `[defaults]`: This section header denotes default configurations for Ansible.

- `host_key_checking=False`: By default, when Ansible connects to a new host for the first time, it prompts the user to
  confirm the host's SSH fingerprint. Setting host_key_checking to False disables this verification, which can be
  helpful for automated processes where manual intervention isn't
  feasible. However, it's worth noting that this makes the connection less secure because it avoids checking for
  potential man-in-the-middle attacks.

- `pipelining=True`: Pipelining reduces the number of SSH operations required to execute a module on the remote server,
  resulting in a significant performance increase. This is particularly beneficial when the connection to the remote
  server has high latency.

- `forks=100`: This determines how many parallel processes Ansible will use when executing commands on remote hosts. A
  higher number can lead to faster execution when managing many hosts, but it also requires more resources on the
  Ansible control node. In this case, it's set to run commands on up
  to 100 hosts simultaneously.

### Clone Kolla-Ansible git repository

On this guide, we are going to use the latest version of Kolla-Ansible, which is `master` branch.

```bash
sudo pip3 install git+https://opendev.org/openstack/kolla-ansible@master
```

To verify the installation, run:

```bash
kolla-ansible --version
```

Result would be like:

```bash
16.1.0
```

**Note:** 16.1.0 is the latest version of Kolla-Ansible at the time of writing this guide.

### Copy Global Configuration Files

kolla-ansible default configuration is a bare minimum configuration, which is not suitable for production
environments. So, we are going to use the example configuration files to configure our environment.

Kolla-Ansible will look for the configuration files in `/etc/kolla` directory. So, we need to copy the
configuration files to that directory.

```bash
sudo mkdir -p /etc/kolla
sudo cp -r /usr/local/share/kolla-ansible/etc_examples/kolla/* /etc/kolla/
```

### Chang owner of the kolla directory to the current user

Kolla-Ansible requires that the user running the deployment commands owns the `/etc/kolla` directory. To change the
owner of the directory to the current user, run:

```bash
sudo chown -R $USER:$USER /etc/kolla
```

### Storage backend configuration

When considering an all-in-one configuration, you have two available choices.

- **LVM on Distinct Disks:**
    - This is the first option available.
    - It involves using LVM (Logical Volume Management) on separate disks or RAIDs.
    - LVM helps manage storage efficiently by creating virtual volumes.
    - Useful for organizing data and optimizing disk usage.
    - This option is **recommended** for all in one scale production environments.

- **Loopback Devices:**
    - This is the second option.
    - It involves using loopback devices, which treat files as disks.
    - Loopback devices can simplify setting up configurations without dedicated disks.
    - Helpful for creating virtual disks within a file on an existing filesystem.
    - This option is recommended for lab environments and testing.
    - This is not a stable option for production environments.

Both options provide ways to set up an all-in-one configuration, each with its benefits and use cases.

We're going to use the first option, which is **LVM on Distinct Disks**.

### Cinder LVM configuration

We are going to use LVM as the backend for Cinder. So, we need to create a new volume group for Cinder.

#### Step A: Identify the disk that you want to use for Cinder.

List all available block devices (like hard drives, SSDs, and their partitions) on the system. This command provides a
structured view which helps in identifying devices by their names, sizes, and mount points.

```bash
lsblk
```

Result would be like:

```bash
loop0                       7:0    0  63.4M  1 loop /snap/core20/1974
loop1                       7:1    0 111.9M  1 loop /snap/lxd/24322
loop2                       7:2    0  53.3M  1 loop /snap/snapd/19457
loop3                       7:3    0  63.5M  1 loop /snap/core20/2015
sda                         8:0    0   931G  0 disk
├─sda1                      8:1    0     1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 927.9G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   927G  0 lvm  /
sdb                         8:16   0   1.7T  0 disk
```

**Points to remember:**

- In this result, we have two disks, `sda` and `sdb`.
- `sda` is the main disk, which is used for the OS,
  and `sdb` is the disk that we are going to use for Cinder.


- Openstack mounts the disk under the name of `vda` and `vdb`, so don't get confused.

**Nice to know:** `sda` or `sdb` stands for SCSI disk A or B.

**You must identify the disk that you want to use for Cinder.**

#### Step B: Create a new physical volumes group for Cinder

**NOTE:** Replace `/dev/sdb` with the disk that you want to use for Cinder.

```bash
sudo pvcreate /dev/sdb
```

Result would be like:

```bash
  Physical volume "/dev/sdb" successfully created.
```

To confirm that the physical volume was created, run:

```bash
sudo pvs
```

You would see something like the following:

```bash
  PV         VG        Fmt  Attr PSize    PFree
  /dev/sda3  ubuntu-vg lvm2 a--  <927.95g 968.00m
  /dev/sdb             lvm2 ---    <1.75t  <1.75t
```

**NOTE:** The values that you would see depends on your system configuration.

#### Step C: Create a new volume group for Cinder.

Create a new volume group named `cinder-volumes` to be used by Cinder:

**NOTE:** Replace `/dev/sdb` with the disk that you want to use for Cinder.

```bash
sudo vgcreate cinder-volumes /dev/sdb
```

Result:

```bash
  Volume group "cinder-volumes" successfully created
```

To confirm that the volume group was created, run:

```bash
sudo vgs
```

result would like the following:

**NOTE:** The values that you would see depends on your system configuration.

```bash
  VG             #PV #LV #SN Attr   VSize    VFree
  cinder-volumes   1   0   0 wz--n-   <1.75t  <1.75t
  ubuntu-vg        1   1   0 wz--n- <927.95g 968.00m
```

## Back to Kolla-Ansible

### Edit globals.yml file

Open `/etc/kolla/globals.yml` in your favorite editor and make the following changes:

DO not use `sudo` here since we have changed the owner of the `/etc/kolla` directory to the current user.

```bash
nano /etc/kolla/globals.yml
```

**Update the following values**

```bash
kolla_base_distro: "ubuntu"
kolla_internal_vip_address: "<YOUR_IP>"
network_interface: "<INTERFACE_NAME>"
neutron_external_interface: "<INTERFACE_NAME_NO_IP>"
enable_haproxy: "no"
enable_cinder: "yes"
enable_cinder_backend_lvm: "yes"
cinder_volume_group: "cinder-volumes"
enable_central_logging: "yes"
enable_aodh: "yes"
enable_ceilometer: "yes"
enable_gnocchi: "yes"
gnocchi_api_public_port: 8041
```

### Configure Passwords

Kolla-Ansible requires passwords for various services. You can generate these passwords using the `kolla-genpwd`
command:

```bash
kolla-genpwd
```

**NOTE:** You will get the following warning:

```bash
WARNING: Passwords file "/etc/kolla/passwords.yml" is world-readable. The permissions will be changed.
```

You can ignore it.

Verify that the passwords were generated successfully by running:

```bash
cat /etc/kolla/passwords.yml
```

This will print the generated passwords to the terminal.

_It's a good idea to save these passwords in a secure location for future reference._

### Setup Ansible Inventory

Copy the inventory file:

```bash
cd ~
cp /usr/local/share/kolla-ansible/ansible/inventory/* .
```

**Nice to know:** The inventory file is a file that contains the list of servers that Kolla-Ansible will manage.

- **All-in-one**: A single node deployment that runs all OpenStack services on a single server.
- **Multinode**: A multi-node deployment that runs OpenStack services on multiple servers.

We are trying to deploy OpenStack on a single server, so we are going to use the `all-in-one` inventory file and we
don't need to change anything.

### Install Ansible Galaxy requirements

Install Ansible Galaxy requirements:

```bash
kolla-ansible install-deps
```

### Install openstack CLI client

Install the OpenStack CLI client:

```bash
sudo pip3 install python-openstackclient
```

### Notes about Ansible

- If you get any error, you can run the command again to fix it.
- `ok` and `changed` values might be different for you, which is normal.
- As long as you see `failed=0`, you are good to go.

### Bootstrap Servers

This step will configure the servers with required packages and configuration:

**NOTE:** You might get a permission error, which is normal. Run any command with `sudo` then run the command again
without `sudo`.

For example run the following command and exit the editor:

```bash
sudo nano a.txt
```

Run the bootstrap command:

```bash
kolla-ansible -i all-in-one bootstrap-servers
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=32   changed=12   unreachable=0    failed=0    skipped=23   rescued=0    ignored=0
```

### Prechecks

Run the `prechecks` to ensure your environment is ready for deployment:

```bash
kolla-ansible -i all-in-one prechecks
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=92   changed=0    unreachable=0    failed=0    skipped=65   rescued=0    ignored=0
```

### Deploy OpenStack

This step might take a while, so be patient, and It's a nice idea to run this command directly from the server console
or KVM, not from SSH.

To deploy OpenStack, run:

```bash
kolla-ansible -i all-in-one deploy
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=382   changed=270    unreachable=0    failed=0    skipped=175   rescued=0    ignored=0
```

### Post Deploy OpenStack

To initialize the OpenStack client environment file:

```bash
kolla-ansible -i all-in-one post-deploy
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=5    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

### Initialize OpenStack Client Config

After a successful deployment, source the OpenStack client environment file:

```bash
source /etc/kolla/admin-openrc.sh
```

### Initialize OpenStack Client Config

use kolla to initialize the openstack environment:

```bash
cd /usr/local/share/kolla-ansible/
sudo ./init-runonce
```

### Post-Deployment

- Create the public and private networks.
- Launch instances and configure security groups.
- Configure any additional services or components as needed.

### Validate Deployment

Check that all containers are up:

```bash
docker ps
```

And try basic OpenStack commands, like:

```bash
openstack image list
openstack network list
```

## Troubleshooting:

- If a deployment fails, you can use `kolla-ansible -i ./all-in-one reconfigure` to attempt to correct it.
- Logs for Kolla-Ansible are typically found in `/var/log/kolla/` but you must access them with `root` user.

### Tips:

- If this is your first time deploying OpenStack, or if you're deploying in a non-production environment, consider
  deploying an "all-in-one" configuration first.
- Before deploying, ensure your servers meet the minimum hardware and software requirements for the roles they'll
  fulfill in your OpenStack environment.
- Ensure your environment's DNS and NTP are configured correctly. OpenStack components are sensitive to time drifts.

Remember, deploying OpenStack can be complex, so it's important to review Kolla-Ansible's official documentation, any
error messages, and logs carefully if you encounter issues.