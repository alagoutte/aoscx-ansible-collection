# module: aoscx_interface

Interface module for Ansible.

Version added: 4.0.0

 - [Synopsis](#Synpsis)
 - [Parameters](#Parameters)
 - [Examples](#Examples)

## Synopsis

This module manages the interface attributes of Aruba AOSCX network devices.

## Parameters

| Parameter         | Type | Choices/Defaults                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Required | Comments                                                                                                                   |
|:------------------|:-----|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------:|:---------------------------------------------------------------------------------------------------------------------------|
| `name`            | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [x]      | Name of the interface. Should be in the format chassis/slot/port e.g. 1/2/3.                                               |
| `enabled`         | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Administrative state of the interface. Use true to administratively enable it.                                             |
| `description`     | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Description of the interface.                                                                                              |
| `configure_speed` | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Option to configure speed/duplex in the interface. If `true`, `autoneg` is required.                                       |
| `autoneg`         | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the auto-negotiation state of the interface. If `false` both `speeds`, and `duplex` are required.                |
| `mtu`             | int  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the MTU value in bytes in the range 46-9198.                                                                     |
| `duplex`          | str  | [`full`, `half`]                                                                                                                                                                                                                                                                                                                                                                                                                                                           | [ ]      | Configure the interface for full duplex or half duplex. If `autoneg` is `on` this must be omitted.                         |
| `speeds`          | list |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the speeds of the interface in megabits per second. If `duplex` is defined only one speed may be specified.      |
| `acl_name`        | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of the ACL being applied or removed from the VLAN.                                                                    |
| `acl_type`        | str  | [`mac`, `ipv4`, `ipv6`]                                                                                                                                                                                                                                                                                                                                                                                                                                                    | [ ]      | Type of ACL being applied or removed from the VLAN.                                                                        |
| `acl_direction`   | str  | [`in`, `out`, `routed-in`, `routed-out`]                                                                                                                                                                                                                                                                                                                                                                                                                                   | [ ]      | Direction for which the ACL is to be applied or removed.                                                                    |
| `qos`             | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of existing QoS configuration to apply to the interface.                                                              |
| `no_qos`          | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Flag to remove the existing Qos of the interface. Use True to remove it.                                                   |
| `queue_profile`   | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of queue profile to apply to interface.                                                                               |
| `qos_trust_mode`  | str  | [`cos`, `dscp`, `name`, `global`]                                                                                                                                                                                                                                                                                                                                                                                                                                          | [ ]      | Specifies the interface QoS Trust Mode. 'global' configures the interface to use the global configuration instead.         |
| `state`           | str  | [`create`, `delete`, `update`]/`create`                                                                                                                                                                                                                                                                                                                                                                                                                                    | [ ]      | The action to be taken with the current Interface.                                                                         |
| `vsx_sync`        | list | [`acl`, `irdp`, `qos`, `rate_limits`, `vlan`, `vsx_virtual`, `virtual_gw_l3_src_mac_enable`, `policy`, `threshold_profile`, `macsec_policy`, `mka_policy`, `portfilter`, `client_ip_track_configuration`, `mgmd_acl`, `mgmd_enable`, `mgmd_robustness`, `mgmd_querier_max_response_time`, `mgmd_mld_version`, `mgmd_querier_interval`, `mgmd_last_member_query_interval`, `mgmd_querier_enable`, `mgmd_mld_static_groups`, `mgmd_igmp_static_groups`, `mgmd_igmp_version`] | [ ]      | Controls which attributes should be synchonized between VSX peers.                                                         |


## Examples

### Configure speed/duplex for an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - full duplex, speed of 1000 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: full
    speeds:
      - 1000
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 1000-full
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - half duplex, speed of 10 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: half
    speeds:
      - 10
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 10-half
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - advertise only 100 Mbps and 1000 Mbps speeds
    and duplex auto-negotiaton.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
    speeds:
      - 100
      - 1000
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m 1g
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - speeds and duplex auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 100-full
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - speeds and duplex auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m
    vlan access 1
```

### Delete configuration of speed/duplex for an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 1000-full
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete full duplex, speed of 1000 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: full
    speeds:
      - 1000
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 10-half
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete half duplex, speed of 10 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: half
    speeds:
      - 10
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m 1g
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete 100 Mbps and 1000 Mbps speeds and
    duplex auto-negotiaton.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
    speeds:
      - 100
      - 1000
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

### Apply ipv4 ACL IN to an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Apply ipv4 ACL to interfaces (new method)
  aoscx_interface:
    name: "1/1/2"
    acl_name: ipv4_acl
    acl_type: ipv4
    acl_direction: in
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    apply access-list ip ipv4_acl in
```

### Administratively disable an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Administratively disable interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    enabled: false
```

After Device Configuration:
```
interface 1/1/2
    shutdown
    vlan access 1
```

### Configure a QoS trust mode

It is possible to set an specific trust mode for a particular interface, or to
configure an interface to use the global default trust mode of the device.

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set a QoS trust mode for interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    qos_trust_mode: cos
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    qos trust cos
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
    qos trust cos
```

Playbook:
```YAML
- name: Set interface 1/1/3 to use global trust mode
  aoscx_interface:
    name: 1/1/3
    qos_trust_mode: global
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Configure a Queue Profile trust mode

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set a Queue Profile for interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    queue_profile: STRICT-PROFILE
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set interface 1/1/3 to use global Queue Profile
  aoscx_interface:
    name: 1/1/3
    use_global_queue_profile: true
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Associate QoS Schedule Profiles to an interface

To assign a Schedule Profile to an interface, you have to specify the name, to
remove it simply use the `no_qos` option.

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Schedule Profile on an interface
  aoscx_interface:
    name: 1/1/2
    qos: STRICT-PROFILE
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    apply qos schedule-profile STRICT-PROFILE
        !actual schedule-profile factory-default
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
    apply qos schedule-profile STRICT-PROFILE
        !actual schedule-profile factory-default
```

Playbook:
```YAML
- name: Remove a Schedule Profile from an interface
  aoscx_interface:
    name: 1/1/3
    no_qos: true
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Set QoS rate for an interface

Before Device Configuration:
```
interface 1/1/17
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set the QoS rate to the 1/1/17 Interface
  aoscx_interface:
    name: 1/1/17
    qos_rate:
      broadcast: 200pps
      unknown_unicast: 100kbps
      multicast: 200pps
```

After Device Configuration:
```
interface 1/1/17
    no shutdown
    vlan access 1
    rate-limit unknown-unicast 100 kbps
    rate_limit broadcast 200 pps
    rate-limit multicast 200 pps
```

### Enable vsx-sync for interface 1/1/2

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - enable vsx-sync features
  aoscx_interface:
    name: 1/1/2
    vsx_sync:
      - acl
      - irdp
      - qos
      - rate_limits
      - vlan
      - vsx_virtual
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vsx-sync access-lists irdp qos rate-limits vlans
    vlan access 1
```
