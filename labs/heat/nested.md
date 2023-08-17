### Advanced - Nested Stacks and Additional Resources

**Objective**: Introduce the concept of nested stacks and include additional resources like volumes and security groups.

**Instructions**:

1. **Creating a Volume Template (Nested Stack)**:
   - Create a file named `volume_template.yaml`.
   - Add the following content:

```yaml
heat_template_version: 2013-05-23

description: Nested stack to create a volume

resources:
  my_volume:
    type: OS::Cinder::Volume
    properties:
      size: 1  # 1 GB

outputs:
  output_volume_id:
    description: ID of the created volume
    value: { get_resource: my_volume }
```

2. **Creating the Main Template with Security Groups and Nested Stack**:
   - Create a new file named `nested_stack.yaml`.
   - Add the following content:

```yaml
heat_template_version: 2021-04-16

description: Main template deploying an instance, security group, and a volume using a nested stack

resources:

  my_security_group:
    type: OS::Neutron::SecurityGroup
    properties:
      rules:
        - protocol: tcp
          port_range_min: 22
          port_range_max: 22
          remote_ip_prefix: 0.0.0.0/0


  volume_stack:
    type: OS::Heat::Stack
    properties:
      template: { get_file: volume_template.yaml }

  my_instance:
    type: OS::Nova::Server
    properties:
      image: "TinyCore"
      flavor: "m1.tiny"
      networks:
        - network: "private"
      security_groups:
        - { get_resource: my_security_group }
      block_device_mapping:
        - device_name: vdb
          delete_on_termination: true
          volume_id: { get_attr: [volume_stack, outputs,output_volume_id] }
```

3. **Deploy the Main Stack with Nested Stack**:
   - Deploy the main stack which in turn deploys the nested stack for volume creation:
     ```bash
     openstack stack create -t nested_stack.yaml nested_main_stack
     ```

4. **Verify the Resources**:
   - Check the list of instances, security groups, and volumes to verify they were created properly.
     ```bash
     openstack server list
     openstack security group list
     openstack volume list
     ```

5. **Clean Up**:
   - Delete the resources:
     ```bash
     openstack stack delete nested_main_stack
     ```

**Explanation**:
- In this lab, we introduced a nested stack (`volume_template.yaml`) which is a separate Heat template that gets called from within the main template. This allows for better organization and reuse of templates.
- The main template (`nested_stack.yaml`) defines an instance, a security group (with an open SSH port), and a call to the nested stack to create a volume.
- We use `OS::Heat::Stack` type to reference another stack (our nested stack in this case) and `template_url` property to point to the nested stack's file.
- The `get_attr` function is used to fetch attributes from resources. In this case, we're fetching the volume's ID from the nested stack to attach it to our instance.
