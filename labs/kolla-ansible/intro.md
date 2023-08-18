### Prerequisites:

1. **Servers**: At least two servers are recommended – one for the control plane and another for compute.
2. **Operating System**: Ubuntu 18.04 LTS or CentOS 7.
3. **SSH key-based authentication** between the deployment node (where you run Kolla-Ansible commands) and the target nodes.
4. **Python**: Ensure Python is installed on all nodes.

### Step-by-step guide:

1. **Install Dependencies**:
    ```bash
    sudo apt-get update && sudo apt install python3-dev libffi-dev gcc libssl-dev python3-pip git
    sudo pip3 install -U pip
    ```

**Install Ansible and Kolla**:
```bash
sudo pip3 install 'ansible>=6,<8'
```
To verify the installation, run:
```bash
ansible --version
```

**Clone Kolla-Ansible git repository** (Optional if installed via pip):
```bash
sudo pip3 install git+https://opendev.org/openstack/kolla-ansible@master
```

To verify the installation, run:

```bash
kolla-ansible --version
```

**Copy Global Configuration**:
```bash
sudo mkdir -p /etc/kolla
sudo cp -r /usr/local/share/kolla-ansible/etc_examples/kolla/* /etc/kolla/
```

** Chang owner of the kolla directory to the current user **

```bash
sudo chown -R $USER:$USER /etc/kolla
```

**Cinder LVM configuration**

Note: This step is only required if you plan to use Cinder with LVM as a backend.

Note:

```bash
lsblk
sudo pvcreate /dev/sdb
sudo vgcreate cinder-volumes /dev/sdb
sudo vgs
```

**Edit `kolla_globals.yml`**:
Open `/etc/kolla/globals.yml` in your favorite editor and make the following changes:

```bash

kolla_base_distro: "ubuntu"
kolla_internal_vip_address: "<YOUR_IP>"
network_interface: "<INTERFACE_NAME>"
neutron_external_interface: "<INTERFACE_NAME_NO_IP>"
enable_neutron_provider_networks: "yes"
enable_openstack_core: "yes"
enable_haproxy: "no"

enable_cinder: "yes"
enable_cinder_backend_lvm: "yes"
cinder_volume_group: "cinder-volumes"

enable_central_logging: "yes"
```

**Configure Passwords**:
```bash
kolla-genpwd
```

**Setup Ansible Inventory**:
Copy the inventory file:
```bash
cp /usr/local/share/kolla-ansible/ansible/inventory/* .
```
Edit the `all-in-one` or `multinode` inventory file based on your setup.

- **All-in-one**: A single node deployment that runs all OpenStack services on a single server.
- **Multinode**: A multi-node deployment that runs OpenStack services on multiple servers.

**Install Ansible Galaxy requirements**

```bash
kolla-ansible install-deps
```

**Bootstrap Servers**:
This step will configure the servers with required packages and configuration:
```bash
kolla-ansible -i all-in-one bootstrap-servers
```

**NOTE:** Run it as sudo once, and then as a normal user.

**Prechecks**:
Run the prechecks to ensure your environment is ready for deployment:
```bash
kolla-ansible -i all-in-one prechecks
```

**Deploy OpenStack**:
```bash
kolla-ansible -i all-in-one deploy
```

**install openstack CLI client**

```bash
sudo pip3 install python-openstackclient
```

**Initialize OpenStack Client Config**:
After a successful deployment, source the OpenStack client environment file:
```bash
source /etc/kolla/admin-openrc.sh
```

**Post-Deployment**:
- Create the public and private networks.
- Launch instances and configure security groups.
- Configure any additional services or components as needed.

**Validate Deployment**:
Check that all containers are up:
```bash
docker ps
```
And try basic OpenStack commands, like:
```bash
openstack image list
openstack network list
```

### Troubleshooting:

- If a deployment fails, you can use `kolla-ansible -i ./all-in-one reconfigure` to attempt to correct it.
- Logs for Kolla-Ansible are typically found in `/var/log/kolla/`.

### Tips:

- If this is your first time deploying OpenStack, or if you're deploying in a non-production environment, consider deploying an "all-in-one" configuration first.
- Before deploying, ensure your servers meet the minimum hardware and software requirements for the roles they'll fulfill in your OpenStack environment.
- Ensure your environment's DNS and NTP are configured correctly. OpenStack components are sensitive to time drifts.

Remember, deploying OpenStack can be complex, so it's important to review Kolla-Ansible's official documentation, any error messages, and logs carefully if you encounter issues.