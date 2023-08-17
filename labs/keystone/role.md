## Lab: Creating Custom Role and Permissions in OpenStack Keystone

**Objective:** Create a custom role in Keystone and assign specific permissions for Nova, Neutron, and Swift.

**Prerequisites:**
- Operational OpenStack environment.
- `openstack` command-line client installed.
- Administrative access to Keystone.

### Step 1: Source Admin Credentials

Source the admin credentials for Keystone access:

```bash
source admin-openrc.sh
```

### Step 2: List Existing Roles

Review existing roles:

```bash
openstack role list
```

### Step 3: Create a New Role

Create a role named `CustomRole`:

```bash
openstack role create MyRole
```

### Step 4: Assign Role to a User and Project

Assign `CustomRole` to user `testuser` for project `MyRoleProject`:

**Note:*** This assumes that `testuser` and `MyRoleProject` already exist.

```bash
openstack role add --user testuser --project MyRoleProject MyRole
```

### PREPARE FOR STEPS 5-6

Create an instance for the project from admin

### Step 5: Set Custom Permissions

**5.1 Nova Permissions**

1. Navigate to `/etc/nova/`.
2. Edit `policy.yaml`.
3. Set policy to allow listing instances:

```json
"os_compute_api:servers:detail": "role:MyRole",
```

**Resource:** [Neutron Policy Reference](https://docs.openstack.org/nova/latest/admin/configuration/samples/policy.yaml.html)

**5.2 Neutron Permissions**

1. Navigate to `/etc/neutron/`.
2. Edit `policy.json`.
3. Set policy to allow listing networks:

```json
"get_network": "role:CustomRole",
```

**Resource:** [Neutron Policy Reference](https://docs.openstack.org/neutron/latest/configuration/policy-sample.html)


### Step 6: Test Custom Role Permissions

1. Source `testuser` credentials:

```bash
source testuser-openrc.sh
```

2. Test Nova permissions:

```bash
openstack server list
```

3. Test Neutron permissions:

```bash
openstack network list
```

4. Test Swift permissions:

```bash
openstack container list
```

For each test, the `testuser` with `CustomRole` should be able to list items, but should encounter permission errors when trying to create or delete.

### Conclusion:

This lab provides a comprehensive walkthrough on how to create custom roles in OpenStack Keystone and assign granular permissions for services like Nova, Neutron, and Swift. Always test thoroughly after making changes to ensure the desired security configuration.