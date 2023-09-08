# Create an OpenStack Image using Packer on Ubuntu

## Duration

Approximately 30 minutes

## Objective

This lab will guide you through the process of creating an image using Packer. Packer is a tool for creating machine images from a single source configuration.

## What is Packer?

Packer is a tool for creating machine images from a single source configuration. Packer is lightweight, runs on every major operating system, and is highly performant, creating machine images for multiple platforms in parallel. Packer does not replace configuration management like Chef or Puppet. In
fact, when building images, Packer is able to use tools like Chef or Puppet to install software onto the image.

## Prerequisites:

- Command line access to the OpenStack environment.

## Step-by-Step Guide:

### Step 1: Install Packer on Ubuntu

For simplicity, we'll install Packer on the same machine as OpenStack. Packer is available for all major operating systems. You can download the latest version of Packer from the [official website](https://www.packer.io/downloads).

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install packer
```

### Step 2: Install Packer Plugin for OpenStack

Packer relies on plugins to communicate with various cloud providers. We'll install the OpenStack plugin for Packer.

```bash
packer plugins install github.com/hashicorp/openstack
```

### Step 3: Create a Packer Template

You can write a template and use it to create your images. A template is a JSON file that describes the image you want to create. The template contains the following sections:

```bash
cd ~
nano ubuntu-image.json
```

content:

```json
{
  "builders": [
    {
      "type": "openstack",
      "source_image": "<SOURCE_IMAGE_ID>",
      "flavor": "<FLAVOR_ID>",
      "networks": [
        "<NETWORK_ID>"
      ],
      "ssh_username": "<Source_Image_Username>",
      "ssh_password": "<Source_Image_Password>",
      "ssh_pty": true,
      "image_name": "<DEST_IMAGE_NAME>",
      "insecure": true
    }
  ],
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "echo 'Hello from Packer provisioner!' > /home/pure/packer_hello.txt"
      ]
    },
    {
      "type": "file",
      "source": "myfile.txt",
      "destination": "/home/pure/myfile.txt"
    }
  ]
}
```

create a file:

```bash
nano myfile.txt
```

content:

```bash
Hello Packer from a file!
```

### Step 4: Source the OpenStack RC file

To make sure that Packer can communicate with OpenStack, we need to source the OpenStack RC file.(You could provide the credentials in the template, but it's not recommended.)

```bash
source /etc/kolla/admin-openrc.sh
```

If you want to add the credentials in your template add the following values to `builders` section

```json
"identity_endpoint": "http://xxx:5000",
"username": "admin",
"password": "xxx",
"domain_name": "Default",
"region": "RegionOne",
```

### Step 5: Validate the Template

To verify that the template is valid, run the following command:

```bash
packer validate ubuntu-image.json
```

result:

```bash
The configuration is valid.
```

### Step 5: Build the Image

To build the image, run the following command:

```bash
packer build ubuntu-image.json
```

Depending on the size of the image, it may take a few minutes to complete. Once the image is created, you can view it in the OpenStack dashboard.

result:

```bash
openstack: output will be in this color.

==> openstack: Loading flavor: xxx
    openstack: Verified flavor. ID: xxx
==> openstack: Creating temporary RSA SSH key for instance...
==> openstack: Not using temporary keypair
==> openstack: Launching server...
==> openstack: Launching server...
    openstack: Server ID: xxx
==> openstack: Waiting for server to become ready...
    openstack: Floating IP not required
==> openstack: Using SSH communicator to connect: xx.xx.xx.xx
==> openstack: Waiting for SSH to become available...
==> openstack: Connected to SSH!
==> openstack: Provisioning with shell script: /tmp/packer-shell612870578
==> openstack: Uploading myfile.txt => /home/pure/myfile.txt
    openstack: myfile.txt 26 B / 26 B [=================================================================================================================================] 100.00% 0s
==> openstack: Stopping server: xxx ...
    openstack: Waiting for server to stop: xxx ...
==> openstack: Creating the image: Packer_image
    openstack: Image: xxx
==> openstack: Waiting for image Packer_image (image id: xxxx) to become ready...
==> openstack: Terminating the source server: xxx ...
Build 'openstack' finished after 1 minute 25 seconds.

==> Wait completed after 1 minute 25 seconds

==> Builds finished. The artifacts of successful builds are:
--> openstack: An image was created: xxx
```

### Step 6: Verify the Image

You can verify that the image is created by running the following command:

```bash
openstack image list
```

result:

```bash
+--------------------------------------+-----------------+--------+
| ID                                   | Name            | Status |
+--------------------------------------+-----------------+--------+
| xxx                                  | Packer_image    | active |
+--------------------------------------+-----------------+--------+
```

## Conclusion

Congratulations! You've successfully created an image using Packer.

## What's next with Packer?

Try to use Jenkins to automate the image creation process using Packer.