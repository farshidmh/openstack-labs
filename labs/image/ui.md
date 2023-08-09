# Creating an Image in OpenStack
Duration: 30 minutes
Objective: Learn how to create a new image that can be used to launch instances.

### Pre-requisites:
- Access to the OpenStack Dashboard
- Download Tiny Core Linux ISO image (http://tinycorelinux.net/14.x/x86/release/TinyCore-current.iso) (23Mb)

### Step-by-Step Guide:

1. **Log in to the OpenStack Dashboard**:
   - Log in with your admin credentials.

2. **Navigate to the Images Section**:
   - Click on `Admin` in the top menu.
   - In the left sidebar, navigate to `Compute` > `Images`.

3. **Create a New Image**:
   - Click on the `Create Image` button.

4. **Configure the Image Details**:
   - **Name**: Enter a name for the image, e.g., "Ubuntu-20.04."
   - **Description**: (Optional) Add a description that helps identify the image.
   - **Image Source**: Choose the source for the image. You can upload a local file or provide a URL.
   - **Format**: Select the appropriate format for the image, such as QCOW2 or RAW. This depends on the source file.
   - **Visibility**: Select whether the image should be public or private.
     - **Public**: The image will be available to all projects.
     - **Private**: The image will be available only to the project that created it.
     - **Shared**: The image will be available to the project that created it and any other projects that have been granted access.
     - **Community**: The image will be available to the project that created it and any other projects that have been granted access. It will also be available to all projects in the community.
   - **Protected**: Choose whether to protect the image from deletion.
   - **Minimum Disk (GB)**: Specify the minimum disk size required for an instance using this image.
   - **Minimum RAM (MB)**: Specify the minimum RAM required for an instance using this image.

5. **Upload the Image**:
   - Click on the `Create Image` button.
   - If you are uploading a local file, a dialog will appear to select the file. Choose the correct file and click `Upload`.

6. **Verify the Image Creation**:
   - Confirm that the new image appears in the images list with the details you've specified.
   - You can click on the image name to view additional details and metadata.

### Summary:
In this lab, you've learned how to create an image in OpenStack. Images are foundational components used to create virtual machines (instances) in the cloud environment. You can create images from various sources and configure them to match specific requirements.

Feel free to practice creating more images with different configurations or move on to exploring other aspects of OpenStack.