### Intermediate - Adding Network Resources to Your Template

**Objective**: Integrate networking components into your Heat template for a more realistic setup.

**Instructions**:

1. **Creating a Heat Template with Networking**:
   - Create a new file named `instance_with_network.yaml`.
   - Add the following content:

```yaml
heat_template_version: 2013-05-23

description: Template to deploy an instance with networking components

resources:

  my_net:
    type: OS::Neutron::Net

  my_subnet:
    type: OS::Neutron::Subnet
    properties:
      network: { get_resource: my_net }
      cidr: 192.168.100.0/24

  my_router:
    type: OS::Neutron::Router

  router_interface:
    type: OS::Neutron::RouterInterface
    properties:
      router_id: { get_resource: my_router }
      subnet_id: { get_resource: my_subnet }

  my_instance:
    type: OS::Nova::Server
    properties:
      image: "TinyCore"
      flavor: "m1.tiny"
      networks:
        - network: { get_resource: my_net }
```

2. **Deploy the Stack with Networking**:
   - Deploy the stack with the following command:
     ```bash
     openstack stack create -t instance_with_network.yaml networked_stack
     ```

3. **Verify Stack and Network Creation**:
   - List the stacks:
     ```bash
     openstack stack list
     ```
   - List the networks to verify your network was created:
     ```bash
     openstack network list
     ```

4. **Launching the Instance**:
   - As before, the instance is launched automatically when the stack is successfully created.

5. **Clean Up**:
   - Clean up the resources:
     ```bash
     openstack stack delete networked_stack
     ```

**Explanation**:
- This template introduces networking components. We've added a network (`my_net`), a subnet (`my_subnet`), and a router (`my_router`). Additionally, a `router_interface` is added to link the subnet with the router.
- For the instance (`my_instance`), the `networks` property is specified to assign the created network to it.
