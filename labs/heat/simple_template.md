### Beginner - Creating Your First Heat Template

**Objective**: Learn how to create a basic Heat template to launch a single instance.

**Instructions**:

1. **Setting up Environment**:
    - Ensure you have the OpenStack command line interface installed.
    - Source the `admin-openrc.sh` (or the appropriate RC file) to set up your environment variables: `source admin-openrc.sh`.

2. **Writing Your First Heat Template**:
    - Create a file named `simple_instance.yaml` using a text editor.
    - Add the following content to the file:

```yaml
heat_template_version: 2021-04-16

description: Simple template to deploy a single compute instance

resources:
   my_instance:
      type: OS::Nova::Server
      properties:
         image: "TinyCore"  #Use a basic small image, ensure it's available in your Glance repository
         flavor: "m1.tiny"  #Use a basic flavor
         networks:
            - network: "public"  # Replace this with the UUID of the network you want to connect to
```

3. **Deploy the Stack**:
    - Using the OpenStack CLI, deploy the stack using the following command:
      ```bash
      openstack stack create -t simple_instance.yaml my_first_stack
      ```

4. **Verify the Stack**:
    - List the stacks to ensure your stack was created:
      ```bash
      openstack stack list
      ```

5. **Clean Up**:
    - Once you've verified your instance, clean up the resources by deleting the stack:
      ```bash
      openstack stack delete my_first_stack
      ```

**Explanation**:
- This basic template defines a single resource (`my_instance`) of type `OS::Nova::Server`, which is essentially a virtual machine instance in OpenStack.
- The `image` and `flavor` properties specify the image and instance type respectively.

---

### Lab 2: Intermediate - Adding Network Resources to Your Template

**Objective**: Learn how to enhance your Heat template to include networking components.
