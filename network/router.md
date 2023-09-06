# Creating a Router in OpenStack Using CLI

## Duration

30 hour for an intro level student.

## Objective

To understand the process of creating a new router in OpenStack using the command line interface and learn how to link it to a network.

## Step-by-Step Guide:

### Step 1: Source the Admin Credentials

To access your OpenStack environment and run CLI commands, source the necessary administrative credentials.

```bash
source admin-openrc.sh
```

### Step 2: Create a Router

Use the following command to create a new router named 'MyRouter'. We'll add the `--enable` flag to ensure that the router is active upon creation.

```bash
openstack router create --enable MyRouter
```

### Step 3: Link the Router to a Network

Now, let's set the router's gateway to an external network (assuming you have one named 'external-net'). This step connects your router to an external network, enabling access to and from instances associated with this router.

```bash
openstack router set --external-gateway external-net MyRouter
```

### Step 4: Add the Subnet to the Router

This step attaches the previously created subnet 'MySubnet' (from Lab 1) to 'MyRouter', facilitating communication between instances in the subnet and external networks.

```bash
openstack router add subnet MyRouter MySubnet
```

### Step 5: Verify the Router Creation and Configuration

List all the routers to confirm the creation and configuration of 'MyRouter'.

```bash
openstack router list
```

To further check the details of 'MyRouter' and ensure the associated networks and subnets are correctly configured, use:

```bash
openstack router show MyRouter
```

## Summary:

In this lab, you learned how to create a new router in OpenStack using CLI, link it to an external network, and attach a subnet. These steps are foundational for setting up a functioning network topology in OpenStack.

---

Before initiating this lab, ensure you've successfully completed [Create Network](cli.md) as it lays the groundwork for this exercise. Additionally, always adapt parameters or values according to your specific OpenStack setup and needs.