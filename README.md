<p align="center"> <img src="https://user-images.githubusercontent.com/34257858/129839002-15e3f2c7-3f75-46d4-afae-0fd207d7fdde.png" width="100" height="100"></p>

<h1 align="center">
    Ansible Role Firewall (<b>iptables</b>)
</h1>

<p align="center" style="font-size: 1.2rem;">
    Installs an iptables-based firewall for Linux. Supports both IPv4 (<b>iptables</b>) and IPv6 (<b>ip6tables</b>).
</p>

<p align="center">

<a href="https://www.ansible.com">
  <img src="https://img.shields.io/badge/Ansible-2.10-green?style=flat&logo=ansible" alt="Ansible">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>
<a href="https://ubuntu.com/">
  <img src="https://img.shields.io/badge/ubuntu-20.x-orange?style=flat&logo=ubuntu" alt="Distribution">
</a>
<a href="https://www.centos.org/">
  <img src="https://img.shields.io/badge/CentOS-8-green?style=flat&logo=centos" alt="Distribution">
</a>

## Requirements

None.

## Role Variables

Dsiable default firewall before setup `iptables`

    iptables_disabled_firewall: true
    iptables_disabled_firewall_services:
    []
    # - ufw.service

Available variables are listed below, along with default values (see `defaults/main.yml`):

    iptables_state: started
    iptables_enabled_at_boot: true

Controls the state of the iptables service; whether it should be running (`iptables_state`) and/or enabled on system boot (`iptables_enabled_at_boot`).

    iptables_flush_rules_and_chains: true

Whether to flush all rules and chains whenever the iptables is restarted. Set this to `false` if there are other processes managing iptables (e.g. Docker).

    iptables_allowed_tcp_ports:
      - "22"
      - "80"
      ...
    iptables_allowed_udp_ports: []

A list of TCP or UDP ports (respectively) to open to incoming traffic.

    iptables_forwarded_tcp_ports:
      - { src: "22", dest: "2222" }
      - { src: "80", dest: "8080" }
    iptables_forwarded_udp_ports: []

Forward `src` port to `dest` port, either TCP or UDP (respectively).

    iptables_additional_rules: []
    iptables_ip6_additional_rules: []

Any additional (custom) rules to be added to the iptables (in the same format you would add them via command line, e.g. `iptables [rule]`/`ip6tables [rule]`). A few examples of how this could be used:

    # Allow only the IP 167.89.89.18 to access port 4949 (Munin).
    iptables_additional_rules:
      - "iptables -A INPUT -p tcp --dport 4949 -s 167.89.89.18 -j ACCEPT"

    # Allow only the IP 214.192.48.21 to access port 3306 (MySQL).
    iptables_additional_rules:
      - "iptables -A INPUT -p tcp --dport 3306 -s 214.192.48.21 -j ACCEPT"

See [Iptables Essentials: Common iptables Rules and Commands](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands) for more examples.

    iptables_log_dropped_packets: true

Set to `false` to disable configuration of ip6tables (for example, if your `GRUB_CMDLINE_LINUX` contains `ipv6.disable=1`).

    iptables_enable_ipv6: true

## Dependencies

None.

## Example Playbook

```yaml
- hosts: server
  vars:
    iptables_allowed_tcp_ports:
      - "22"
      - "25"
      - "80"
  roles:
    - { role: asapdotid.iptables }
```

## TODO

- Make outgoing ports more configurable.
- Make other iptables features (like logging) configurable.

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
