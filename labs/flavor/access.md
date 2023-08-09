# Managing Flavor Access Control**
Duration: 60 minutes
Objective: Learn how to restrict access to specific flavors so that only certain projects can use them.

### Step-by-Step Guide:

1. **Log in to the OpenStack Dashboard**:
   - Log in with your admin credentials.

2. **Navigate to the Flavors Section**:
   - Click on `Admin` in the top menu.
   - In the left sidebar, navigate to `System` > `Flavors`.

3. **Choose a Flavor to Restrict**:
   - Locate the flavor you wish to manage (e.g., "cli-flavor" from Lab 4).
   - Click on the dropdown menu (usually represented by three dots) next to the flavor.
   - Select `Update Metadata`.

4. **Add a Project to the Access List**:
   - In the "Update Flavor Metadata" dialog, you can add specific project IDs to the access list.
   - You can find the project ID by navigating to `Identity` > `Projects`, and then clicking on the project's name.
   - Add the project ID to the access list and click on `Update Metadata`.

5. **Verify the Access Control**:
   - To confirm that the flavor is accessible only to the specified project, log in as a user from a different project and try to create an instance with the restricted flavor. You should find that the flavor is not available.

6. **(Optional) Remove the Access Restriction**:
   - If you wish to remove the access restriction later, simply go back to the `Update Metadata` dialog for the flavor and remove the project ID from the access list.

### Summary:
In this lab, you learned how to manage flavor access control, allowing you to restrict specific flavors to certain projects. This can be a valuable tool in managing resources and controlling how different projects utilize the available virtual machine configurations.

Feel free to explore other ways to control flavor access or manage other OpenStack components.