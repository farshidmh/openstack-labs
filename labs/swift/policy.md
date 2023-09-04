## Managing Storage Policies in OpenStack Swift via CLI

### Objective:

In this lab, we will explore the concept of storage policies in OpenStack Swift. By the end, you'll understand how to create, apply, and manage storage policies.

### Time Estimate:

You should be able to complete this lab in less than `30 minutes`.

### Prerequisites:

- An operational OpenStack environment with the Swift service deployed.
- Previous completion of Lab 1 and Lab 2 or a foundational understanding of OpenStack Swift operations.

### Instructions:

## Create New Project

For an organized and isolated environment, always start with a fresh project and user setup.

Create a dedicated project and user for this specific lab:

```bash
openstack project create MyPolicyProject
openstack user create --password policypassword policyuser
openstack role add --project MyPolicyProject --user policyuser member
```

Use the `openrc` file corresponding to this new project for authentication:

```bash
source MyPolicyProject-openrc.sh
```

**Understanding Storage Policies** 

in Swift are rules or behaviors applied to containers, determining where and how their objects are stored. Policies can influence data availability, durability, and storage locations.

**Creating a Storage Policy**  
While Swift provides default storage policies, often you'll need custom ones. To create a new storage policy (note: this step is typically done by administrators and requires changes in Swift's configuration files):

- Edit `/etc/swift/swift.conf`.
```bash
sudo nano /etc/swift/swift.conf
```

- Under [storage-policy:X], where X is a new unique number, add a name for the policy, e.g., name = SSDStorage.
```bash
[storage-policy:1]
name = SSDStorage
default = no
aliases = faststorage, ssdpolicy
```







- Restart the Swift services.
```bash
sudo systemctl restart devstack@s-*
```



**Applying a Storage Policy to a Container**  
When creating a new container, apply a storage policy using the `--storage-policy` flag:

```bash
swift post --storage-policy=Gold mygoldcontainer
```

**Verifying the Policy of a Container**  
To check which policy is applied to a container:

```bash
swift stat mygoldcontainer
```

**Changing a Container's Policy**  
Currently, OpenStack Swift doesn't allow changing a container's policy directly. Instead, you'd create a new container with the desired policy, migrate the data, and then delete the old container.

```bash
swift post --storage-policy=Silver mysilvercontainer
swift copy --destination mysilvercontainer mygoldcontainer
swift delete mygoldcontainer
```

### Conclusion:

This lab introduced the concept of storage policies in OpenStack Swift. Understanding and leveraging storage policies is crucial for efficient storage management, ensuring data durability, and optimizing costs in large-scale storage systems.

---

Whenever you're ready for the next lab, please let me know!