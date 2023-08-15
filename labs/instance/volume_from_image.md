Certainly! Here's how you can create a bootable volume from an image and then launch an instance from that volume using the OpenStack CLI:

### Prerequisites:
1. OpenStack CLI tools installed (`openstack` command).
2. Necessary credentials sourced (usually via an `openrc` file).
3. An available image in the OpenStack Glance (for this guide, we'll assume an image named "Ubuntu-20.04").

### Lab Instructions:

1. **Source your OpenStack credentials**:
   ```bash
   source path_to_your_openrc_file
   ```

2. **Create a Volume from an Image**:

   List available images:
   ```bash
   openstack image list
   ```

   Using the ID of "centos", create the volume:
   ```bash
   openstack volume create --size 10 --image [IMAGE_ID] CentOSVolumeFromImage
   ```

   Here, replace `[IMAGE_ID]` with the ID of the "Ubuntu-20.04" image. The volume will be 10 GiB in size.

3. **Verify the Volume is Bootable**:

   ```bash
   openstack volume show CentOSVolumeFromImage
   ```

   Ensure that the "bootable" property reads "true".

4. **Launch an Instance from the Bootable Volume**:

   First, gather some necessary details:
    - List flavors to choose an appropriate size for the instance:
      ```bash
      openstack flavor list
      ```
    - List networks to choose a network for the instance:
      ```bash
      openstack network list
      ```

   Using the volume ID, flavor, and network, create the instance:
   ```bash
   openstack server create --flavor [FLAVOR_ID] --nic net-id=[NETWORK_ID] --volume [VOLUME_ID] VolumeBootInstance
   ```

   Replace `[FLAVOR_ID]`, `[NETWORK_ID]`, and `[VOLUME_ID]` with appropriate values from previous steps.

5. **Verify the Instance is Running**:

   ```bash
   openstack server list
   ```

   Ensure that "VolumeBootInstance" is in the list and its status is "ACTIVE".

6. **Accessing the Instance** (Optional):

   If you have set up key pairs and security groups with SSH access, you can SSH into the instance using its floating IP or private IP.

7. **Clean Up**:

   If you wish to delete the instance and volume (after your tasks are complete):
   ```bash
   openstack server delete VolumeBootInstance
   openstack volume delete UbuntuVolumeFromImage
   ```

That's the step-by-step guide for creating a bootable volume from an image and launching an instance using that volume in OpenStack via the CLI. Remember to replace placeholders with actual values from your environment.