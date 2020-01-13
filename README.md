# Ansible Role: keepalived

Installs and configure a basic keepalived service on RedHat/CentOS Linux servers.

## Requirements

None.

## Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    vrrp_state: "MASTER"

The initial state being the `MASTER` or `SLAVE`. 

    vrrp_interface: "{{ ansible_facts['default_ipv4']['interface'] }}"

interface for inside_network, bound by vrrp.

    vrrp_priority: 100

The peer election priority.

    virtual_ipaddress: "192.168.122.100"

The virtual ipaddress added/removed from change to MASTER.

    unicast_src_ip: "{{ ansible_facts['default_ipv4']['address'] }}"

The default IP for binding vrrpd is the primary IP.

    unicast_peer: "192.168.122.102"

The peer receiving VRRP adverts

## Dependencies

None.

## Example Playbook 
[![keepalived.png](https://i.postimg.cc/c18zvcRV/keepalived.png)](https://postimg.cc/mhBjqC6w)
### Keepalive MASTER peer

    - hosts: haproxy_master
      sudo: yes
      vars:
        vrrp_state: "MASTER"
        vrrp_interface: "{{ ansible_facts['default_ipv4']['interface'] }}"
        vrrp_priority: 100
        virtual_ipaddress: "192.168.122.100"
        unicast_src_ip: "{{ ansible_facts['default_ipv4']['address'] }}"
        unicast_peer: "192.168.122.102"
      roles:
        - { role: stephenturns.haproxy }
### Keepalive SLAVE peer

    - hosts: haproxy_slave
      sudo: yes
      vars:
        vrrp_state: "SLAVE"
        vrrp_interface: "{{ ansible_facts['default_ipv4']['interface'] }}"
        vrrp_priority: 50
        virtual_ipaddress: "192.168.122.100"
        unicast_src_ip: "{{ ansible_facts['default_ipv4']['address'] }}"
        unicast_peer: "192.168.122.101"
      roles:
        - { role: stephenturns.haproxy }

## License

MIT / BSD

## Author Information

This role was created in 2020 by [Stephen Turner](https://github.com/stephenturns).
