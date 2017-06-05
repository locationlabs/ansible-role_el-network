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

    el_network_interfaces:
      - iface: ens32
        type: ethernet
        ip4: '192.168.0.10/24'
        gw4: '192.168.0.1'
      - iface: ens33
        type: bond-slave
        master: bond0
      - iface: bond0
        type: bond
        ip4: '10.0.0.10/24'
        bonding_mode: 4

Then simply run the role:

    - hosts: servers
      roles:
        - el-network
