### Advanced - Auto-Scaling with Heat

**Objective**: Set up an auto-scaling group of instances using Heat that can grow or shrink based on CPU utilization.

**Instructions**:

1. **Define the Scaling Policy and Alarm**:
    - Create a file named `autoscaling_template.yaml`.
    - Add the following content:

```yaml
heat_template_version: 2013-05-23

description: Auto-scaling template to manage a group of instances based on CPU utilization

resources:

  scale_up_policy:
    type: OS::Heat::ScalingPolicy
    properties:
      adjustment_type: change_in_capacity
      auto_scaling_group_id: { get_resource: instance_group }
      cooldown: 60
      scaling_adjustment: 1

  scale_down_policy:
    type: OS::Heat::ScalingPolicy
    properties:
      adjustment_type: change_in_capacity
      auto_scaling_group_id: { get_resource: instance_group }
      cooldown: 60
      scaling_adjustment: -1

  cpu_alarm_high:
    type: OS::Aodh::Alarm
    properties:
      meter_name: cpu_util
      threshold: 70.0
      comparison_operator: gt
      statistic: avg
      period: 60
      evaluation_periods: 1
      alarm_actions:
        - { get_attr: [scale_up_policy, alarm_url] }

  cpu_alarm_low:
    type: OS::Aodh::Alarm
    properties:
      meter_name: cpu_util
      threshold: 30.0
      comparison_operator: lt
      statistic: avg
      period: 60
      evaluation_periods: 1
      alarm_actions:
        - { get_attr: [scale_down_policy, alarm_url] }

  instance_group:
    type: OS::Heat::AutoScalingGroup
    properties:
      min_size: 1
      max_size: 5
      resource:
        type: OS::Nova::Server
        properties:
          image: "cirros"
          flavor: "m1.tiny"
          networks:
            - network: "private"
```

2. **Deploy the Auto-scaling Stack**:
    - Deploy the stack with:
      ```bash
      openstack stack create -t autoscaling_template.yaml autoscale_stack
      ```

3. **Test Auto-scaling**:
    - Monitor the instances using:
      ```bash
      openstack server list
      ```
    - Simulate high CPU usage (depending on the instance type, this may involve running compute-intensive operations or benchmarking tools). Once the CPU utilization goes above 70% (the threshold we've set), a new instance should be spawned.
    - Similarly, if you reduce the CPU usage and it goes below 30%, an instance should be terminated.

4. **Clean Up**:
    - Delete the stack and all associated resources:
      ```bash
      openstack stack delete autoscale_stack
      ```

**Explanation**:
- The `OS::Heat::AutoScalingGroup` resource is used to define a group of identical instances that can be scaled up or down.
- The `OS::Heat::ScalingPolicy` resource describes the scaling action, such as growing or shrinking the group by a fixed number.
- The `OS::Aodh::Alarm` resources define the criteria (CPU usage in this case) for triggering the scaling actions. In this example, when average CPU usage goes above 70%, the group is scaled up, and when it goes below 30%, the group is scaled down.

Auto-scaling is vital for maintaining performance and availability in response to varying loads. With Heat, it's easy to define scaling behavior and associate it with cloud resources.