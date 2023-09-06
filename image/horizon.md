# Creating an Image in OpenStack Using Horizon

## Duration

30 minutes for intro level students

### Pre-requisites:

- Access to the OpenStack Dashboard
- Download Tiny Core Linux ISO image (http://tinycorelinux.net/14.x/x86/release/TinyCore-current.iso) (23Mb)

## Objective

To familiarize students with the process of creating a new virtual machine image in OpenStack using the Horizon dashboard, which can later be used to launch instances.

## Step-by-Step Guide:

### Step 1: Access OpenStack Horizon Dashboard

Ensure you're connected to the internet and have the necessary credentials at hand.

### Step 2: Navigate to the Images Section

- After logging in, click on `Admin` in the top menu.
- From the left sidebar, select `Compute` followed by `Images`.

### Step 3: Initiate Image Creation

Click on the `Create Image` button located in the Images section.

### Step 4: Provide Image Details

- **Name**: Assign a name to the image, such as "TinyCore"
- **Description**: Optionally, furnish a brief description for ease of identification.
- **Image Source**: Decide if you'll upload a local file or use a URL.
- **Format**: Pick the right format, be it QCOW2 or RAW, depending on your source.
- **Visibility**: Designate who can access the image, with options including public, private, shared, and community.
- **Protected**: Decide if you'd like to prevent unintentional deletion.
- **Minimum Disk (GB)**: Define the least amount of disk space required by an instance using this image.
- **Minimum RAM (MB)**: Set the minimum RAM requirement for an instance using this image.

### Step 5: Confirm Image Upload

Hit the `Create Image` button. If uploading a file from your system, select the right file when the dialog box appears, and then click `Upload`.

### Step 6: Validate Image Creation

Check that your new image is visible in the list, displaying the details you provided. For more insights, click on the image name.

## Summary:

Through this lab, you've gained hands-on experience in creating a virtual machine image using OpenStack's Horizon dashboard. This is a foundational skill in cloud computing, enabling you to launch instances tailored to specific needs. You can further practice by setting up more images with varying
configurations or delve deeper into other facets of OpenStack.