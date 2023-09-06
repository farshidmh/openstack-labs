# Creating a New Flavor in OpenStack using UI

## Duration

approximately 30 minutes

## Objective

Understand what flavors are and learn how to create a new flavor in OpenStack.

### Understanding Flavors:

Flavors define the compute, memory, and storage capacity of Nova computing instances. They are essential when defining the size and nature of virtual machines in OpenStack.

## Step-by-Step Guide:

### Step 1: Log in to the OpenStack Dashboard:

Log in with an admin account to access the admin functionalities.

### Step 2: Navigate to the Flavors Section:

In the left sidebar, navigate to `Admin` > `Compute` > `Flavors`.

### Step 3: View Existing Flavors:

Here you can see a list of all the flavors available in your OpenStack deployment. Take note of the properties of a couple of flavors, such as vCPUs, RAM, and disk size.

### Step 4: Create a New Flavor:

Click on the `Create Flavor` button.

Fill in the required details:

- **Name**: Enter a name, such as "micro-flavor."
- **VCPUs**: Set the number of virtual CPUs, such as 1.
- **RAM MB**: Set the amount of RAM in MB, such as 512.
- **Root Disk GB**: Set the root disk size in GB, such as 5.
- **Ephemeral Disk GB**: Set the ephemeral disk size in GB, such as 0.
- **Swap Disk MB**: Set the swap disk size in MB, such as 0.
- **RXTX Factor**: Set the RXTX factor, such as 1.0.
- **ID (optional)**: You can leave this blank, and OpenStack will generate an ID for you.
-

Click on `Create Flavor`.

### Step 5: Verify the Flavor Creation*:

    - Confirm that the new flavor appears in the flavors list with the details you've specified.

## Summary:

In this lab, you learned what flavors are in OpenStack and how to create a new flavor using the admin dashboard. You can use this new flavor to launch instances with the specified configuration.

Feel free to practice creating a few more flavors with different configurations or move on to the next lab when you're ready.