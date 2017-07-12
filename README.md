# el-network

Ansible role for managing network interfaces on enterprise linux.

## Requirements

* netaddr

## Role Variables

* `el_network_interfaces`: List of interfaces
* `el_network_whitelist_ifaces`: List of whitelisted interfaces to not remove. Typically lo and idrac.
* `el_network_configured_ifaces`: Auto-generated variable that is a list of interfaces on the target host to configure.

## Dependencies

N/A

## Example Playbook

Example vars setup:

```yaml
el_network_interfaces:
  - iface: ens32
    type: ethernet
    ip4: '192.168.0.10/24'

  - iface: ens33
    type: bond-slave
    master: bond0

  - iface: bond0
    type: bond
    bonding_mode: 4
    bridge: br0

  - iface: bond0.10
    type: ethernet
    vlan: yes

  - iface: br0
    type: bridge
    ip4: '10.0.0.10/24'
    gw4: '10.0.0.1'
```

Then simply run the role:

```yaml
- hosts: servers
  roles:
    - el-network
```

---

## OVS

Simple OVS support exists to integrate an interface and/or bridge into OVS. **Note:** This role does
not install OVS, that is left up to the admin.

```yaml
el_network_interfaces:
  - iface: ens33
    type: ethernet
    bridge: br-ex
    ovs: yes

  - iface: br-ex
    type: bridge
    ovs: yes
    ip4: '172.16.0.10/24'
```
