# Lab: Creating and Resizing an OpenStack Volume using CLI - Beginner Level

### Objective:
In this lab, you will first learn how to create a new OpenStack volume. After successfully creating the volume, you will resize it using the OpenStack command line interface (CLI).

### Prerequisites:
- Access to an OpenStack environment.
- OpenStack CLI tools installed.
- Necessary credentials to authenticate with OpenStack.

### Lab Steps:

#### Step: Authenticate with OpenStack

```bash
source openrc_file
```
> **Explanation:** `openrc_file` is a script that sets environment variables for communication with your OpenStack cloud. This file has authentication details such as project name, username, password, etc.

#### Step: Create a New Volume

To create a new volume, use the following command:

```bash
openstack volume create --size INITIAL_SIZE VOLUME_NAME
```
> **Example:** To create a 20GB volume named "MyNewVolume", the command would be:
```bash
openstack volume create --size 1 MyNewVolume
```
> **Explanation:** This command instructs OpenStack to create a new volume with the specified name and initial size.

#### Step: List Existing Volumes

```bash
openstack volume list
```
> **Explanation:** This command lists all volumes in your project. After creating a new volume, it's a good practice to list volumes and confirm that your new volume is indeed created and available.

#### Step: Choose the Volume to Resize

Identify the volume's ID or name you want to resize from the list of volumes.

> **Example:** Let's assume the volume ID of "MyNewVolume" is `12345-6789-abcd`.

#### Step: Resize the Volume

```bash
openstack volume set --size NEW_SIZE VOLUME_ID_OR_NAME
```
> **Example:** To resize the volume to 50GB, the command would be:
```bash
openstack volume set --size 5 MyNewVolume
```
> **Explanation:** This command tells OpenStack to resize the volume with the given ID or name to the new size.

#### Step: Verify the Changes

```bash
openstack volume show VOLUME_ID_OR_NAME
```
> **Example:**
```bash
openstack volume show MyNewVolume
```
> **Explanation:** This command fetches detailed information about the specified volume, including its size. Check the "size" attribute to confirm the volume has been resized.

#### Step: Additional Considerations

If the volume is attached to a running instance, resizing the volume might require you to also resize the filesystem inside it. This can involve using tools like `resize2fs` (for ext4 filesystems) within the OS of the instance the volume is attached to.