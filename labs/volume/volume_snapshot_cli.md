# Launching an Instance with a Volume

## Objective:

In this lab, you'll learn how to take a snapshot of a running instance and then use that snapshot to boot a new instance. This is useful for creating backups or clones of instances.

## Time Estimate:

You should be able to complete this lab in less than `45 minutes`.

## Prerequisites:

- OpenStack CLI tools installed.
- `openrc` file available.

## Steps:

### Access the OpenStack CLI

Follow `Source Ubuntu` to source the `openrc` file.

### Create a Project (Tenant)

Create a project (tenant) for the user to work in:

```bash
openstack project create MyProject
```

### Create a User

Create a user and assign a password:

```bash
openstack user create --password mypassword myuser
```

### Assign the User to the Project

Assign the user to the project (tenant):

```bash
openstack role add --project MyProject --user myuser member
```

### Add an Image

Follow [Convert Image](image/convert_image.md) to create crrios image.

__It should be called: `CirrOs 0.6.2-x86_64`__


### **IMPORTANT** Switch source to the new user

Based on instructor's instructions, get the rc of the new user and switch context.


### Create a Network and Subnet

In order to launch an instance, you need to create a network and subnet for the user to work with:

```bash
openstack network create MyNetwork 
openstack subnet create --network MyNetwork --subnet-range 192.168.10.0/24 MySubnet
```

### Create a Volume

Create an empty volume in your project (tenant):

```bash
openstack volume create --size 5 MyVolume
```

### Launch an Instance

```bash
openstack server create --image MyImage --flavor FLAVOR_ID --nic net-id=MyNetwork  MyInstance
```

Based on the following labs, replace `IMAGE_ID`, `FLAVOR_ID`, and `NETWORK_ID` with the appropriate IDs/names from your environment.
- [List Images](../image/list_images.md)
- [List Flavors](../flavor/list_flavors.md)
- [List Networks](../network/list_networks.md)

Wait for the instance to become ACTIVE before moving to the next step. You can check the instance status using openstack server list.


### Attach the Volume to the Instance

Attach the volume that you created to the instance:

```bash
openstack server add volume MyInstance MyVolume
```

**Note:** It may take a few minutes for the volume to attach to the instance.


### Verify the Volume Attachment

Console into the instance and run:

```bash
lsblk
```

You should see a new volume attached to the instance.


### Snapshot the Running Instance

Create a snapshot of the running instance. This snapshot will be saved as an image in the OpenStack Image service (Glance).

```bash
openstack server image create --name InstanceSnapshot MyInstance
```

8. Boot a New Instance from the Snapshot

Use the created snapshot to boot a new instance.

```bash
openstack server create --image InstanceSnapshot --flavor FLAVOR_ID --nic net-id=MyNetwork ClonedInstance
```

9. Verify the New Instance

SSH into the new instance, `ClonedInstance`, to verify its content and configuration. It should mirror the state of `InitialInstance` at the time of the snapshot.

## Clean-Up:

### Delete the Instances

```bash
openstack server delete MyInstance
openstack server delete ClonedInstance
```

### Delete the Snapshot Image

```bash
openstack image delete InstanceSnapshot
```

## Remove the Initial Image

```bash
openstack volume delete MyVolume
```

## Delete the Network and Subnet

```bash
openstack subnet delete MySubnet
openstack network delete MyNetwork
```

## Switch Back

Switch back to admin user and continue with the following steps.

## Delete the User and Project

   ```bash
   openstack user delete myuser --project MyProject
   openstack project delete MyProject
   ```

**Note:** You might be stayed in the project of the user you just deleted. but you can't interact with it. This is a known bug in OpenStack dashboard.

## Verify Deletion

   ```bash
   openstack server list
   openstack network list
   openstack subnet list
   openstack user list
   openstack project list
   ```

### Conclusion:

In this comprehensive lab, you learned how to set up a basic OpenStack environment, launch an instance with a volume, create a snapshot of it and subsequently clean up all created resources.