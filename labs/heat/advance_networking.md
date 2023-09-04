### Lab 6: Networking with Heat

**Objective**: Create a private network, router, and a VM connected to this network using OpenStack Heat.

**Instructions**:

1. **Define the Networking Template**:
   - Create a file named `networking_template.yaml`.
   - Add the following content:

```yaml
heat_template_version: 2013-05-23

description: Networking Template to create a private network, router, and an instance connected to the network.

resources:

  private_network:
    type: OS::Neutron::Net
    properties:
      name: my_private_network

  private_subnet:
    type: OS::Neutron::Subnet
    properties:
      network_id: { get_resource: private_network }
      cidr: 192.168.100.0/24
      gateway_ip: 192.168.100.1
      dns_nameservers:
        - 8.8.8.8
        - 8.8.4.4

  router:
    type: OS::Neutron::Router
    properties:
      external_gateway_info:
        network: "public"   # Adjust this if your external network has a different name.

  router_interface:
    type: OS::Neutron::RouterInterface
    properties:
      router_id: { get_resource: router }
      subnet_id: { get_resource: private_subnet }

  my_instance:
    type: OS::Nova::Server
    properties:
      image: "cirros"
      flavor: "m1.tiny"
      networks:
        - network: { get_resource: private_network }
```

2. **Deploy the Network Stack**:
   - Deploy the stack with:
     ```bash
     openstack stack create -t networking_template.yaml network_stack
     ```

3. **Verify the Deployment**:
   - Check that the network, subnet, and router have been created:
     ```bash
     openstack network list
     openstack subnet list
     openstack router list
     ```
   - Verify that the instance is up and running and connected to the private network:
     ```bash
     openstack server list
     ```

4. **Access the VM**:
   - If your cloud setup allows, try to access the VM, for example using SSH (Note: This step might require additional configurations like floating IPs, security groups, etc., which are not covered in this basic lab).

5. **Clean Up**:
   - Delete the stack and all associated resources:
     ```bash
     openstack stack delete network_stack
     ```

**Explanation**:

- In this lab, you learned how to use Heat to orchestrate the creation of basic networking resources in OpenStack.
- `OS::Neutron::Net` creates a private network.
- `OS::Neutron::Subnet` creates a subnet within the specified private network.
- `OS::Neutron::Router` sets up a router, and with `OS::Neutron::RouterInterface`, you attach the subnet to the router.
- Finally, `OS::Nova::Server` is used to spin up an instance connected to the private network.

This lab showcases the power of Heat to orchestrate multiple resources across different OpenStack services in a coordinated manner.