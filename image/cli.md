# Creating an Image in OpenStack Using CLI

## Duration

Approximately 30 minutes

## Objective

To understand and execute the process of generating an image through the OpenStack CLI.

## Pre-requisites:

- ISO or qcow2 of the image you want to create
    - For this lab, we will use Ubuntu Server image as an
      example (https://releases.ubuntu.com/22.04.3/ubuntu-22.04.3-live-server-amd64.iso)
    - **Note:** You don't need to download the image

## Step-by-Step Guide:

### Step 1: Access the Terminal

Follow [Access CLI](../access_rc_file.md) to access the openstack CLI.

### Step 2:  Download an Image (if you don't already have one locally):

For this lab, we will use Ubuntu image as an example:

```bash
curl -L 'https://releases.ubuntu.com/22.04.3/ubuntu-22.04.3-live-server-amd64.iso' -o  Ubuntu-server-22-04.iso
```

**Heads up**: Before initiating download, ensure the ISO file URL is valid. In case this link is unavailable, you can always find the latest on the [ubuntu server download page](https://ubuntu.com/download/server).

### Step 4: Create a New Image:

Run the following command to create a new image using the downloaded file:

```bash
openstack image create --public --disk-format iso "Ubuntu ISO" --file Ubuntu-server-22-04.iso
```

Adjust the flags and values according to your requirements:

- `--public`: Makes the image accessible to all users.
- `--disk-format`: Specifies the disk format (e.g., "qcow2", "raw").
- `"Ubuntu ISO"`: The name of the image.
- `--file`: The path to the image file. (Absolute path is recommended)

### Step 5: Verify the Image Creation:

List the available images and confirm that your new image is in the list:

```bash
openstack image list
```

result would be something like this:

```bash
+--------------------------------------+------------+--------+
| ID                                   | Name       | Status |
+--------------------------------------+------------+--------+
| b7d1dc62-6606-4f4a-a47d-8438a88e9d8d | TinyCore   | active |
| 459b08ac-dcd1-4a1e-84d5-b05530d1c341 | Ubuntu ISO | active |
| a4c9c136-9dc7-47c9-bc7f-69cb0aab7a20 | cirros     | active |
+--------------------------------------+------------+--------+
```

**Points to Remember**:

- The `Status` column should be `active`.
- List of available images may vary depending on your environment.

### Step 6: Delete the Image (Optional)

If you want to delete the image, run the following command:

```bash
openstack image delete "Ubuntu ISO"
```

## Summary:

In this lab, you've learned how to create an image in OpenStack using the CLI. This approach is powerful, particularly
for automation and scripting tasks. You've seen how to download an image and then create an OpenStack image from that
file.

Feel free to experiment by creating images with different formats and attributes.