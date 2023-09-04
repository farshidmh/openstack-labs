**Lab 2: Creating and Managing Users**

### Objective:
This lab will guide you through the process of creating and managing users within OpenStack. By the end of this lab, you'll understand how to create, list, and delete users using the OpenStack CLI.

### Prerequisites:
- Lab 1 should be completed.
- Command line access to the OpenStack environment.

### Steps:

1. **List current users**

   ```bash
   openstack user list
   ```

    - **Explanation**: This command gives you an overview of all existing users in your OpenStack environment.

2. **Create a new user**

   To create a new user, you'll need a username and password at the minimum. We'll create a user named 'testuser' with a password 'testpass123'.

   ```bash
   openstack user create --password testpass123 testuser
   ```

    - **Explanation**: This command will create a user named `testuser` with the specified password.

3. **Set an email for the user**

   Users can also have additional properties like an email. We'll set an email for 'testuser'.

   ```bash
   openstack user set --email testuser@example.com testuser
   ```

    - **Explanation**: This sets the email for `testuser` to `testuser@example.com`.

4. **Verify the user's properties**

   After creating and setting properties for the user, you can show the details of the user.

   ```bash
   openstack user show testuser
   ```

    - **Explanation**: This command displays the properties and details of `testuser`.

5. **Delete the user**

   If you wish to delete the user, use the following command:

   ```bash
   openstack user delete testuser
   ```

    - **Explanation**: This command deletes `testuser` from OpenStack. Make sure to proceed with caution, as this action is irreversible.

### Conclusion:
You have successfully created a user, modified its properties, viewed its details, and deleted it. Managing users is a foundational task in OpenStack, ensuring that resources are accessed and used by the right entities.

---

Let me know when you're ready for Lab 3.