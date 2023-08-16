## Advanced Object Operations in OpenStack Swift via CLI

### Objective:

This lab delves deeper into OpenStack Swift, focusing on advanced object operations. By the end, you'll understand how to set access control on objects, work with object metadata, and perform bulk operations.

### Time Estimate:

You should be able to complete this lab in less than `30 minutes`.

### Prerequisites:

- An operational OpenStack environment with the Swift service deployed.
- Previous experience or completion of Lab 1.

### Instructions:

## Create New Project

Working in a dedicated project environment helps in focusing on specific tasks without any external disruptions.

Start by creating a dedicated project and user for this lab:

```bash
openstack project create MyAdvancedProject
openstack user create --password advancedpassword advanceduser
openstack role add --project MyAdvancedProject --user advanceduser member
```

Follow your instructor's steps to generate an `openrc` file for this new project and use it for authentication:

```bash
source MyAdvancedProject-openrc.sh
```

**Setting Object Access Controls**  
One of Swift's key features is its fine-grained access control. Let's set read-only permissions for everyone on `myfile.txt`:

```bash
swift post -r ".r:*,.rlistings" mycontainer
swift upload mycontainer myfile.txt
```

**Working with Object Metadata**  
Swift allows attaching metadata to objects, aiding in categorization or adding supplementary information. Add metadata to `myfile.txt`:

```bash
swift post mycontainer --meta "Description:Sample File" --meta "Version:1.0" myfile.txt
```

Inspect the metadata:

```bash
swift stat mycontainer myfile.txt
```

**Uploading Multiple Files**  
Swift provides a way to upload multiple files at once, a useful feature when dealing with large datasets. If you have several files (`file1.txt`, `file2.txt`, `file3.txt`), upload them together:

```bash
echo "file1" > file1.txt
echo "file2" > file2.txt
echo "file3" > file3.txt
swift upload mycontainer file1.txt file2.txt file3.txt
```

**Downloading Multiple Files**  
Similarly, Swift supports bulk downloads. Fetch all the files you uploaded in the previous step:

```bash
swift download mycontainer file1.txt file2.txt file3.txt
```

**Bulk Deletion**  
To delete multiple objects from Swift in a single command, use:

```bash
swift delete mycontainer file1.txt file2.txt file3.txt
```

**Deleting the Container**  
Before deleting a container, ensure it's empty. An attempt to delete a container with objects will fail. Here, we'll delete the now-empty `mycontainer`:

```bash
swift delete mycontainer
```

### Conclusion:

This lab provided a deeper exploration of object operations in OpenStack Swift. You learned how to manage access controls, work with metadata, and conduct bulk operations. These skills empower you to manage storage in OpenStack Swift effectively and efficiently.

---

When you're ready to proceed to the next lab, please let me know!