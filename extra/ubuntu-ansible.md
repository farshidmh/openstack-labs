# How to setup LVM on Ubuntu 22.04 for Cinder

## Duration

Approximately 20 minutes

## Objective

To understand and learn how to setup LVM on Ubuntu 22.04.

## Prerequisites:

1. **Hardware**: 2 Hard Disks.
2. **Operating System**: Ubuntu 22.04 LTS.

## Important Notes:

- This should be done on the deployment node.
- This is a step-by-step guide, so you must follow it in the same order.
- This guide is tested on Ubuntu 22.04 LTS.

## Step-by-step guide:

### Step A: Install Ansible

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

### Step B: Edit Ansible configuration file

Create Ansible directory:

```bash
sudo mkdir /etc/ansible
```

Create Ansiible configuration file:

```bash
sudo nano /etc/ansible/ansible.cfg
```

Contents of the file should be:

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

## Summary

In this guide, you learned how to install Ansible and configure it for Kolla Ansible.