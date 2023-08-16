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

Follow [Convert Image](../image/convert_image.md) to create crrios image.

__It should be called: `CirrOs 0.6.2-x86_64`__


### **IMPORTANT** Switch source to the new user

Based on instructor's instructions, get the rc of the new user and switch context.


### Create a Network and Subnet

In order to launch an instance, you need to create a network and subnet for the user to work with:

```bash
openstack network create MyNetwork 
openstack subnet create --network MyNetwork --subnet-range 192.168.10.0/24 MySubnet
```

### Launch an Instance

```bash
openstack server create --image MyImage --flavor FLAVOR_ID --nic net-id=MyNetwork  MyResizableInstance
```

**Note:** Choose a flavor that is small.

Based on the following labs, replace `IMAGE_ID`, `FLAVOR_ID`, and `NETWORK_ID` with the appropriate IDs/names from your environment.
- [List Images](../image/list_images.md)
- [List Flavors](../flavor/list_flavors.md)
- [List Networks](../network/list_networks.md)

Wait for the instance to become ACTIVE before moving to the next step. You can check the instance status using openstack server list.

### Resize the Instance

Resize the instance to a larger flavor:

```bash
openstack server resize --flavor FLAVOR_ID MyResizableInstance
```

Wait for a few moments for the instance to complete the resizing process.


### Confirm the Resize

Get list of instances:

```bash
openstack server list
```

Once the instance is in a `VERIFY_RESIZE` state, you need to confirm the resize for the changes to take effect permanently.

```bash
openstack server resize confirm MyResizableInstance
```

### Verify the Instance Size

SSH into the instance or check its details to verify that the resources have been updated according to the new flavor.

## Clean-Up:

### Delete the Instances

```bash
openstack server delete MyResizableInstance
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

In this lab, you learned how to resize an instance to a different flavor and then confirm the resize. This allows you to dynamically adjust resources for an instance based on evolving requirements.
