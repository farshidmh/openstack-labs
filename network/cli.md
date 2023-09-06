# Creating a Network in OpenStack Using CLI

## Duration

30 minutes for an intro level student.

## Objective

To understand the process of creating a new network in OpenStack using the command line interface and familiarize oneself with network parameters.

## Step-by-Step Guide:

### Step 1: Source the Admin Credentials

Before you can interact with the OpenStack environment, ensure that you have the necessary administrative credentials loaded.

```bash
source admin-openrc.sh
```

### Step 2: Create a Network

Use the following command to create a new network named 'MyNet'. 

```bash
openstack network create MyNet
```

### Step 3: Create a Subnet

After creating the network, let's add a subnet to it. This command will create a subnet 'MySubnet' in the 'MyNet' network with the IP range `192.168.0.0/24`.

```bash
openstack subnet create --network MyNet --subnet-range 192.168.0.0/24 MySubnet
```

### Step 4: Verify the Network and Subnet Creation

List all the networks to confirm the creation of 'MyNet'.

```bash
openstack network list
```

Similarly, list all the subnets to verify the creation of 'MySubnet'.

```bash
openstack subnet list
```

### Step 5: Delete the Network and Subnet (Optional)

If you want to delete the network and subnet, use the following commands:

```bash
openstack subnet delete MySubnet
openstack network delete MyNet
```


## Summary:

In this lab, you learned how to create a new network and subnet in OpenStack using the CLI. You also got familiar with some network parameters and their implications.
