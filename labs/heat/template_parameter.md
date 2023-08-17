### Advanced - Parameterized Heat Template

**Objective**: Learn how to make Heat templates dynamic by introducing parameters.

**Instructions**:

1. **Creating a Parameterized Heat Template**:
    - Create a new file named `parameterized_template.yaml`.
    - Add the following content:

```yaml
heat_template_version: 2013-05-23

description: Template with parameters to allow dynamic input

parameters:
  instance_flavor:
    description: Flavor for the Nova instance
    type: string
    default: m1.tiny

resources:

  my_instance:
    type: OS::Nova::Server
    properties:
      image: "TinyCore"
      flavor: { get_param: instance_flavor }
      networks:
        - network: "private"  # Assuming a 'private' network exists
```

2. **Deploy the Stack Using Parameters**:
    - Deploy the stack specifying a parameter:
      ```bash
      openstack stack create -t parameterized_template.yaml --parameter instance_flavor=m1.small param_stack
      ```

3. **Verify the Instance**:
    - List the instances to check the instance flavor:
      ```bash
      openstack server list
      ```

4. **Clean Up**:
    - Delete the resources:
      ```bash
      openstack stack delete param_stack
      ```

**Explanation**:
- This template introduces the `parameters` section. We've defined an `instance_flavor` parameter that allows users to specify the flavor dynamically during stack creation.
- The `get_param` function retrieves the value of the parameter.
