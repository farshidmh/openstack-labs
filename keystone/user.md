# Creating and Managing Users

## Duration

Approximately 30 minutes

## Objective

This lab will guide you through the process of creating and managing users within OpenStack. By the end of this lab, you'll understand how to create, list, and delete users using the OpenStack CLI.

## Prerequisites:

- Command line access to the OpenStack environment.

## Step-by-Step Guide:

### Step 1: List current users

This command gives you an overview of all existing users in your OpenStack environment.

```bash
openstack user list
```

### Step 2: Create a new user

To create a new user, you'll need a username and password at the minimum. We'll create a user named 'testuser' with a password 'testpass123'.

 ```bash
 openstack user create --password testpass123 testuser
 ```

### Step 3: Set an email for the user

Users can also have additional properties like an email. We'll set an email for 'testuser'.

```bash
openstack user set --email testuser@example.com testuser
```

### Step 4: Verify the user's properties

After creating and setting properties for the user, you can show the details of the user.

```bash
openstack user show testuser
```

### Step 5: Delete the user (optional)

If you wish to delete the user, use the following command:

```bash
openstack user delete testuser
```

## Conclusion:

You have successfully created a user, modified its properties, viewed its details, and deleted it. Managing users is a foundational task in OpenStack, ensuring that resources are accessed and used by the right entities.
