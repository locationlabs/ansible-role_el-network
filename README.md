# el-network

Ansible role for managing network interfaces on enterprise linux.

## Requirements

* netaddr

**Note**: This role isn't compatible with firewalld

## Role Variables

* `el_network_interfaces`: List of interfaces
	* `iface`: Name of the interface
	* Common interface options:
		* `ip4`: IPv4 address in CIDR notation
		* `gw4`: Default gateway
		* `bridge`: Name of bridge interface this is a member of
	* `type`: Type of interface
		* ethernet
			* `dhcp`: Enable dhcp
			* `vlan`: Enable vlan support
		* bond-slave
			* `master`: Name of parent bond interface
		* bond
			* `bonding_mode`: Default value 4 - 802.3ad
			* `lacp_rate`: Default value 1 - fast
			* `xmit_hash_policy`: Default value layer3+4
			* `miimon`: Default value 100
		* bridge
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
