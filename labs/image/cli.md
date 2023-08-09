# Creating an Image in OpenStack Using CLI
Duration: 60 minutes
Objective: Learn how to create an image using the OpenStack command-line interface.

### Pre-requisites:
- Access to the OpenStack CLI
- Ubuntu ISO image url (https://releases.ubuntu.com/22.04.2/ubuntu-22.04.2-desktop-amd64.iso?_ga=2.20057848.1383591010.1691591029-2094439131.1691591029) 
  - Note: You don't need to download the image
- wget 
  - Note: If you don't have wget installed, you can install it with `yum install wget -y` or `apt install wget -y`

### Step-by-Step Guide:

1. **Access the Terminal**:
   - Open a terminal window on the system where the OpenStack CLI is installed.

2. **Source the OpenStack RC File**:
   - To interact with your OpenStack deployment, you need to source the RC file containing the environment variables.
   - Run the following command:
     ```bash
     source keystonerc_admin
     ```

3. **Download an Image** (if you don't already have one locally):
   - For this lab, we will use Ubuntu image as an example:
   - **IMPORTANT**: You must get the download link from ubuntu
     ```bash
     curl -L 'https://releases.ubuntu.com/22.04.2/ubuntu-22.04.2-desktop-amd64.iso' -o  Ubuntu.iso
     ```

4. **Create a New Image**:
   - Run the following command to create a new image using the downloaded file:
     ```bash
     openstack image create --public --disk-format iso "Ubuntu" --file Ubuntu.iso
     ```

   - Adjust the flags and values according to your requirements:
     - `--public`: Makes the image accessible to all users.
     - `--disk-format`: Specifies the disk format (e.g., "qcow2", "raw").
     - `"Ubuntu"`: The name of the image.
     - `--file`: The path to the image file.

5. **Verify the Image Creation**:
   - List the available images and confirm that your new image is in the list:
     ```bash
     openstack image list
     ```

### Summary:
In this lab, you've learned how to create an image in OpenStack using the CLI. This approach is powerful, particularly for automation and scripting tasks. You've seen how to download an image and then create an OpenStack image from that file.

Feel free to experiment by creating images with different formats and attributes.