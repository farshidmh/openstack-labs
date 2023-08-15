## **OpenStack Lab: Installing ISO into Volume and Booting from the Volume**

### **Prerequisites:**
- OpenStack environment up and running.
- OpenStack command line tools installed and authenticated against your environment.
- A valid ISO file (e.g., a Linux distribution ISO).

## Time Estimate:

You should be able to complete this lab in less than `45 minutes`.


### **Lab Procedure:**

**Source your OpenStack RC file:**  
Before executing OpenStack commands, authenticate your session.
```bash
source your-openstack-rc-file.sh
```

**Upload ISO to Glance:**  
This command uploads your ISO file to the OpenStack Image service (Glance).
```bash
openstack image create "IMAGE_NAME" --file IS_FILE --disk-format iso --public
```

**Create a volume:**  
To store the contents of the ISO, create a new volume. Adjust the size based on your ISO's requirements.
```bash
openstack volume create --size 10 ISO_Volume
```

**Boot an instance from the ISO:**  
Using the uploaded ISO, start an instance. This instance behaves similarly to a machine booting from a physical CD/DVD.
```bash
openstack server create --flavor m1.large --image IMAGE_NAME  --nic net-id=Your_Net_ID MyInstance
```
**NOTE:** Do not forget to network the instance.

**NOTE:** Do not forget to attach the volume to the instance.

**HINT**

```bash
openstack server add volume MyInstance ISO_Volume
```

**Accessing the Instance:**
- If your ISO is a live CD, you can access the system immediately.
- If it's an installation ISO, use the instance console via Horizon or another method to continue the installation process. Make sure to install the OS onto the volume you've created.

**After setup is complete:**

- shut down the instance
- detach the volume
**HINT**
```bash
openstack server remove volume MyInstance ISO_Volume
```


**Make the volume bootable:**  
After the OS installation onto the volume, mark the volume as bootable so it can be used for new instances.
```bash
openstack volume set --bootable ISO_Volume
```

**Boot a new instance from the volume:**  
You can now start a new instance using the bootable volume. This instance will retain the data from the volume, even after termination.
```bash
openstack image create --volume <volume_name> <your_image_name>
```

**Convert the volume to an image:**
If you want to use the volume as an image, you can convert it to an image using the following command:
```bash
openstack volume upload --volume ISO_Volume --image-name New_Image_Name
```


---

### **Conclusion:**
You've now successfully uploaded an ISO to OpenStack, established a bootable volume from that ISO, and launched an instance using the bootable volume. This technique is especially useful when deploying an OS or applications available only in ISO format in your OpenStack environment.