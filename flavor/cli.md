# Creating a Flavor Using the OpenStack CLI

## Duration

Approximately 30 minutes

## Objective

Learn how to create a flavor using the OpenStack CLI.

## Step-by-Step Guide:

### Step 1: Access the Terminal:

Follow [Access CLI](../access_rc_file.md) to access the openstack CLI.

### Step 2: Create the Flavor:

Run the following command to create a new flavor named "ubuntu-test-flavor" with 1 vCPU, 1024 MB of RAM, and 10 GB of
disk
space:

```bash
openstack flavor create --ram 1024 --disk 10 --vcpus 1 "ubuntu-test-flavor"
```

### Step 3: Verify the Creation:

Run the following command to view the list of flavors and confirm that "cli-flavor" is in the list:

```bash
openstack flavor list
```

result would be something like this:

```bash
+--------------------------------------+--------------------+-------+------+-----------+-------+-----------+
| ID                                   | Name               |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------------------+--------------------+-------+------+-----------+-------+-----------+
| 1                                    | m1.tiny            |   512 |    1 |         0 |     1 | True      |
| 2                                    | m1.small           |  2048 |   20 |         0 |     1 | True      |
| 3                                    | m1.medium          |  4096 |   40 |         0 |     2 | True      |
| 31762b23-7da0-4888-80cf-ffb7c9f0c253 | ubuntu-test-flavor |  1024 |   10 |         0 |     1 | True      |
| 4                                    | m1.large           |  8192 |   80 |         0 |     4 | True      |
| 5                                    | m1.xlarge          | 16384 |  160 |         0 |     8 | True      |
+--------------------------------------+--------------------+-------+------+-----------+-------+-----------+
```

**Remember:** List of flavors may vary depending on your OpenStack installation.

### Step 4: Delete the Flavor (Optional):

To delete the flavor, run the following command:

```bash
openstack flavor delete "ubuntu-test-flavor"
```

### Bonus: Explore Additional Options:

You can explore additional options for creating a flavor by running:

```bash
openstack flavor create --help
```

## Summary:

In this lab, you've learned how to create a flavor using the OpenStack CLI. This method offers a programmatic way to
create and manage flavors and can be especially useful in automating deployments and configurations.

Feel free to practice creating additional flavors with different configurations using the CLI.