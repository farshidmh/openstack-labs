# Launching an Instance with a Volume

## Objective:

Set up a basic OpenStack environment by creating a project, user, network, and image. Subsequently, create a volume, launch an instance, attach the volume, and then clean up all created resources.

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

SSH into the instance and run:

   ```bash
   lsblk
   ```

You should see a new volume attached to the instance.

Feel free to create a file system on the volume and mount it.
```bash
vgcreate vg01 /dev/vdb
lvcreate -L 4G -n lv01 vg01
vgdsplay
```

## Clean-Up:

## Detach the Volume from the Instance

   ```bash
   openstack server remove volume MyInstance MyVolume
   ```

## Delete the Instance

   ```bash
   openstack server delete MyInstance
   ```

## Delete the Volume

   ```bash
   openstack volume delete MyVolume
   ```

## Remove the Image

   ```bash
   openstack image delete MyImage
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

## Verify Deletion

   ```bash
   openstack server list
   openstack volume list
   openstack image list
   openstack network list
   openstack subnet list
   openstack user list
   openstack project list
   ```

#### Conclusion:

In this comprehensive lab, you learned how to set up a basic OpenStack environment, launch an instance with a volume, and subsequently clean up all created resources.