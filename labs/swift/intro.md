## Introduction to OpenStack Swift via CLI

### Objective:

In this lab, we'll introduce you to the basics of using OpenStack Swift through the Command Line Interface (CLI). By the end, you'll understand how to install the OpenStack Swift client, authenticate with your OpenStack environment, and perform basic CRUD operations on Swift containers and objects.

### Time Estimate:

You should be able to complete this lab in less than `30 minutes`.

### Prerequisites:

- An operational OpenStack environment with Swift service deployed.
- User credentials (typically found in an `openrc` file) to access the OpenStack environment.

### Instructions:

**Installing the OpenStack Swift Client**  
The OpenStack Swift client provides a command-line interface to interact with the Swift Object Storage service. It can be installed via `apt`:

```bash
sudo apt  install python3-swiftclient -y
```

**Authenticating with OpenStack Swift**  
Before executing Swift commands, it's essential to authenticate with the OpenStack environment. This is typically done by sourcing the `openrc` file, which sets various environment variables used for authentication.

```bash
source openrc admin admin
```

## Create new project

Create dedicated project and user for this lab

```bash
openstack project create MyProject
openstack user create  --password mypassword myuser
openstack role add --project MyProject --user myuser member
```

Follow instructor's to create `openrc` file for this project and use it to authenticate

**Creating a Container**  
Containers are logical storage units in Swift where objects (files) are stored. Think of them like directories in a traditional file system. Here, we'll create a container named `mycontainer`:

```bash
swift post mycontainer
```

**Verifying Container Creation**  
After creating a container, it's a good practice to verify its existence. List all your containers:

```bash
swift list
```

**Uploading a File to the Container**  
To store files in Swift, you'll upload them to a specific container. For this example, we'll upload a file named `myfile.txt` to the `mycontainer`:

```bash
echo "Hello World" > myfile.txt
swift upload mycontainer myfile.txt
```

**Listing Objects within the Container**  
To see the contents of a container, you can list the objects it contains. This command will show us the objects in `mycontainer`:

```bash
swift list mycontainer
```

**Retrieving the Uploaded File**  
Downloading files from Swift is straightforward. The following command fetches `myfile.txt` from the `mycontainer` and saves it in your current working directory:

```bash
cd ~
mkdir myfiles/
cd myfiles/
swift download mycontainer myfile.txt
```

**Deleting the File from the Container**  
To delete an object from a container, use the `delete` command. This will remove `myfile.txt` from `mycontainer`:

```bash
swift delete mycontainer myfile.txt
```

**Deleting the Container**  
Before deleting a container, ensure it's empty. An attempt to delete a container with objects will fail. Here, we'll delete the now-empty `mycontainer`:

```bash
swift delete mycontainer
```

### Conclusion:

In this lab, you were introduced to the fundamental operations of OpenStack Swift using the CLI. From creating containers to manipulating objects within them, you now have a foundational understanding of how to interact with Swift's Object Storage service.

---

Whenever you're ready for the next lab, please let me know!