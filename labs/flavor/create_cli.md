# Creating a Flavor Using the OpenStack CLI

**Duration:** 30 minutes
**Objective:** Learn how to create a flavor using the OpenStack CLI.

### Step-by-Step Guide:

1. **Access the Terminal**:

   - Open a terminal window on the system where the OpenStack CLI is installed.
2. **Source the OpenStack RC File**:

   - You need to have access to the environment variables to interact with your OpenStack deployment.
   - Source the RC file by running:
     ```bash
     source keystonerc_admin
     ```
3. **Create the Flavor**:

   - Run the following command to create a new flavor named "cli-flavor" with 1 vCPU, 1024 MB of RAM, and 10 GB of disk space:
     ```bash
     openstack flavor create --id auto --ram 1024 --disk 10 --vcpus 1 "cli-flavor"
     ```
4. **Verify the Creation**:

   - Run the following command to view the list of flavors and confirm that "cli-flavor" is in the list:
     ```bash
     openstack flavor list
     ```
5. **(Optional) Explore Additional Options**:

   - You can explore additional options for creating a flavor by running:
     ```bash
     openstack flavor create --help
     ```

### Summary:

In this lab, you've learned how to create a flavor using the OpenStack CLI. This method offers a programmatic way to create and manage flavors and can be especially useful in automating deployments and configurations.

Feel free to practice creating additional flavors with different configurations using the CLI.