## Lab 2: Managing Domains with Keystone

### Objective

Explore the domain concept in OpenStack Keystone and learn how to create, manage, and delete domains using the CLI.

### Steps

- **Login with the new user and project:**


- **Introduction to Domains:**

  Domains in OpenStack act as containers for projects, users, and groups. They can be used for multiple purposes, such as isolating users between different organizations or dividing resources by department.

- **Create a new domain:**

  Begin by creating a domain named `lab2domain`:

  ```bash
  openstack domain create lab2domain
  ```

- **List all available domains:**

  Confirm the creation of your domain:

  ```bash
  openstack domain list
  ```

  You should see `lab2domain` in the list of available domains.

- **Create a project within the domain:**

  Now that your session is set within the `lab2domain`, create a project named `InsideDomainProject`:

  ```bash
  openstack project create --domain lab2domain InsideDomainProject
  ```

- **List projects within the domain:**

  View all the projects under your domain:

  ```bash
  openstack project list --domain lab2domain
  ```

  `InsideDomainProject` should appear in the list.

- **Delete the project and domain:**

  First, delete the `InsideDomainProject`:

  ```bash
  openstack project delete InsideDomainProject
  ```

  Then, remove the `lab2domain`:

  ```bash
  openstack domain set --disable lab2domain
  openstack domain delete lab2domain
  ```

### Conclusion

In this lab, you learned about domains in OpenStack, which provide a way to organize and isolate users, projects, and other resources. You created, managed, and deleted domains and projects within them. Understanding domains is crucial as you scale and manage more complex OpenStack deployments.