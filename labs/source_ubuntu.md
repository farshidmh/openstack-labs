# Source openrc file in Ubuntu 20.04 LTS created by devstack installation.

## Objective:

In this lab you will learn how to source the `openrc` file in Ubuntu 20.04 LTS created by devstack installation.

## Time Estimate:

You should be able to complete this lab in less than `15 minutes`.

## Prerequisites:

- `devstack` installed.

## Steps:

### Change User

by default, the `stack` user is created during the installation of `devstack` and the `openrc` file is available in the root directory of the `stack` user.

To gain access to the `stack` user, run the following command:

```bash
sudo su - stack
cd devstack
```

### Source the OpenRC File

To setup the environment variables for the Openstack CLI tools to work properly, source the `openrc` file:

```bash
source openrc admin admin
```

## Conclusion:

Congratulations! You have successfully sourced the `openrc` file in Ubuntu 20.04 LTS created by devstack installation. now you can use the Openstack CLI tools to manage your Openstack environment.
