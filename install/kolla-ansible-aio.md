# Install Kolla-Ansible All-in-One (AIO)

## Duration

Approximately 2 hours

## Objective

To understand and install OpenStack using Kolla-Ansible All-in-One (AIO) on Ubuntu 22.04.

## Prerequisites:

1. **Servers**: One server with at least 8GB of RAM and 4 CPU cores. This server will be managed by Kolla-Ansible and
   will run all OpenStack services.
2. **Operating System**: Ubuntu 22.04 LTS.
3. **Disk**: One disk with at least 50GB of free space for OS and one disk with at least 100GB of free space for Cinder.
4. **Network**: Two network interfaces connected to the same network and same gateway.(Second interface must not have an IP address/Valid IP address)

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

### Step A: Install Dependencies

Kolla-Ansible relies on some Python libraries that are not available in the default Ubuntu setup, so you need to install
them.

Update the package list:

```bash
sudo apt-get update
```

Install the required packages:

```bash
sudo apt install python3-dev libffi-dev gcc libssl-dev python3-pip git
```

Install the latest version of pip:

```bash
sudo pip3 install -U pip
```

### Step B: Install Ansible

Follow the instructions in [How to setup Ansible on Ubuntu 22.04 for Kolla-Ansible](../extra/ubuntu-ansible.md) to install Ansible.

### Step C: Install Kolla-Ansible from source

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

Find release notes at [Kolla Ansible Release Notes documentation](https://docs.openstack.org/releasenotes/kolla-ansible/)

### Step D: Copy Global Configuration Files

kolla-ansible default configuration is a bare minimum configuration, which is not suitable for production environments. So, we are going to use the example configuration files to configure our environment.

Kolla-Ansible will look for the configuration files in `/etc/kolla` directory. So, we need to copy the configuration files to that directory.

```bash
sudo mkdir -p /etc/kolla
```

```bash
sudo cp -r /usr/local/share/kolla-ansible/etc_examples/kolla/* /etc/kolla/
```

### Step E: Chang owner of the kolla directory to the current user

Kolla-Ansible requires that the user running the deployment commands owns the `/etc/kolla` directory. To change the
owner of the directory to the current user, run:

```bash
sudo chown -R $USER:$USER /etc/kolla
```

### Step F: Storage backend configuration

Follow the instructions in [How to setup LVM on Ubuntu 22.04 for Cinder](../extra/ubuntu-lvm.md) to setup LVM for Cinder.

### Step G: Edit globals.yml file

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

Explanation of the values:

- `kolla_base_distro: "ubuntu"`
  This defines the base Linux distribution on which Kolla will deploy its containerized services. In this case, the distribution specified is Ubuntu.

- `kolla_internal_vip_address: "<YOUR_IP>"`
  This is the IP address used for the Virtual IP (VIP) within the cluster. This address is typically used for accessing cluster services. You need to replace `<YOUR_IP>` with the desired IP address.

- `network_interface: "<INTERFACE_NAME>"`
  This refers to the network interface name that Kolla will use for communication. `<INTERFACE_NAME>` should be replaced with the actual name of the network interface (e.g., eth0, ens3, etc.)

- `neutron_external_interface: "<INTERFACE_NAME_NO_IP>"`
  This specifies the network interface name that Neutron (the network component of OpenStack) will use for external communication. This interface should not have an IP address. Replace `<INTERFACE_NAME_NO_IP>` with the appropriate interface name.

- `enable_haproxy: "no"`
  Indicates whether the HAProxy service should be enabled or not. In this case, it's set to "no", which means HAProxy will not be deployed.

- `enable_cinder: "yes"`
  Determines if the Cinder service, which provides block storage in OpenStack, should be enabled. Here, it's set to "yes".

- `enable_cinder_backend_lvm: "yes"`
  Indicates whether the LVM (Logical Volume Manager) backend for Cinder should be enabled. In this instance, it's set to "yes".

- `cinder_volume_group: "cinder-volumes"`
  This defines the name of the volume group that Cinder will use for provisioning block storage. The default is typically "cinder-volumes".

- `enable_central_logging: "yes"`
  Determines if centralized logging for Kolla services should be enabled. Here, it's set to "yes".

- `enable_aodh: "yes"`
  Indicates if the Aodh service, an alarming service in OpenStack, should be enabled. It's set to "yes" here.

- `enable_ceilometer: "yes"`
  Specifies if the Ceilometer service, which provides telemetry data in OpenStack, should be enabled. Here, it's set to "yes".

- `enable_gnocchi: "yes"`
  Determines if the Gnocchi service, a time-series database for metrics in OpenStack, should be enabled. In this case, it's set to "yes".

- `gnocchi_api_public_port: 8041`
  This sets the public port number for the Gnocchi API service. In this instance, it's set to 8041.

Note: Always refer to the official documentation or guidelines when configuring any software, this is simplification and might not capture the complete depth or latest changes related to Kolla or OpenStack settings.

### Step H: Configure Passwords

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

### Step I: Setup Ansible Inventory

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

### Step J: Install Ansible Galaxy requirements

Install Ansible Galaxy requirements:

```bash
kolla-ansible install-deps
```

### Step K: Install openstack CLI client

Install the OpenStack CLI client:

```bash
sudo pip3 install python-openstackclient
```

### Notes about Ansible

- If you get any error, you can run the command again to fix it.
- `ok` and `changed` values might be different for you, which is normal.
- As long as you see `failed=0`, you are good to go.

### Step L: Bootstrap Servers

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

### Step M: Prechecks

Run the `prechecks` to ensure your environment is ready for deployment:

```bash
kolla-ansible -i all-in-one prechecks
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=92   changed=0    unreachable=0    failed=0    skipped=65   rescued=0    ignored=0
```

### Step N: Deploy OpenStack

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

### Step P: Post Deploy OpenStack

To initialize the OpenStack client environment file:

```bash
kolla-ansible -i all-in-one post-deploy
```

Result would be like:

```bash
PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=5    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

### Step Q: Initialize OpenStack Client Config

After a successful deployment, source the OpenStack client environment file:

```bash
source /etc/kolla/admin-openrc.sh
```

### Step R: Initialize OpenStack Client Config

use kolla to initialize the openstack environment:

```bash
cd /usr/local/share/kolla-ansible/
sudo ./init-runonce
```

### Step S: Post-Deployment

- Create the public and private networks.
- Launch instances and configure security groups.
- Configure any additional services or components as needed.

### Step T: Validate Deployment

Check that all containers are up:

```bash
docker ps
```

And try basic OpenStack commands, like:

```bash
openstack image list
openstack network list
```

### Step U: Access Horizon Dashboard

Interact with your openstack environment using the Horizon dashboard or RC file.

## Troubleshooting:

- If a deployment fails, you can use `kolla-ansible -i ./all-in-one reconfigure` to attempt to correct it.
- Logs for Kolla-Ansible are typically found in `/var/log/kolla/` but you must access them with `root` user.

### Tips:

- If this is your first time deploying OpenStack, or if you're deploying in a non-production environment, consider
  deploying an "all-in-one" configuration first.
- Before deploying, ensure your servers meet the minimum hardware and software requirements for the roles they'll
  fulfill in your OpenStack environment.
- Ensure your environment's DNS and NTP are configured correctly. OpenStack's components are sensitive to time drifts.

Remember, deploying OpenStack can be complex, so it's important to review Kolla-Ansible's official documentation, any
error messages, and logs carefully if you encounter issues.