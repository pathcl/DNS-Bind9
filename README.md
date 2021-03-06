# DNS-Bind9

[![Build Status](https://travis-ci.org/mrLarbi/DNS-Bind9.svg?branch=master)](https://travis-ci.org/mrLarbi/DNS-Bind9)
[![Github License](https://img.shields.io/aur/license/yaourt.svg?maxAge=2592000)](LICENSE)
[![Ansible Role](https://img.shields.io/badge/ansible--galaxy-mrLarbi.dns--bind-blue.svg)](https://galaxy.ansible.com/mrLarbi/dns-bind/)

## Description

**[Ansible role](https://galaxy.ansible.com/mrLarbi/dns-bind/)** to install and configure a DNS server using Bind9.
The node installed will act as a master or/and a slave, with/without forwarders.

## Requirements

- Ansible 1.4+

## Dependencies

None.

## Installation

```
ansible-galaxy install mrLarbi.dns-bind
```

## Role Variables

### General

|Name|Default|Description|
|----|----|-------|
bind_hostname||The node hostname. Defined automatically if not set.
bind_domain_name|local|The node domain. Defined automatically if not set.
bind_ip||The node's ip. Defined automatically if not set.
bind_listen_on|{"ip":"any", port:""}|Listening addresses and port. {"ip":"@someip", "port":"@someport"} format. The ip attribute can take the following values : any, none, localhost, localnet or an ip sequence (Example : "127.0.0.1; 192.168.0.1").
bind_listen_on_v6|{"ip":"none", port:""}|Listening addresses for IPv6 and port. {"ip":"@someip", "port":"@someport"} format. The ip attribute can take the following values : any, none, localhost, localnet or an ip sequence (Example : "::1;").


### Master

|Name|Default|Description|
|----|----|-------|
bind_is_master|no| Is the node a master. 'yes' or 'no'.
bind_mail||The zone email. Defined automatically if not set (to $user.$domain).
bind_master_entries|| Zone entries. List of {"name":"@something", "ip": "@someip"}.
bind_slave_hosts|| List of slaves (Allow transfer).
bind_notify|no| 

### Slave

|Name|Default|Description|
|----|----|-------|
bind_is_slave|no| Is the node a slave. 'yes' or 'no'.
bind_master_host|| Master ip.

### Caching

|Name|Default|Description|
|----|----|-------|
bind_is_caching|no| Is a caching node. 'yes' or 'no'
bind_first_forwarder|| Ip of the first forwarder.
bind_second_forwarder|| Ip of the second forwarder.

## Configuration example

Set up a master node :

      bind_hostname: "yoda"
      bind_domain_name: "jedi"
      bind_ip: "192.168.1.2"
      bind_mail: "yoda.jedi"
      bind_is_master: yes
      bind_master_entries : [ {"name": "www", "ip":"127.0.0.1"} ]
      bind_slave_hosts: [ "192.168.1.3" ]
      bind_notify: yes

Set up a slave node :

      bind_hostname: "yoda"
      bind_domain_name: "jedi"
      bind_ip: "192.168.1.3"
      bind_is_slave: yes
      bind_master_host: "192.168.1.2"
    
Set up a caching node :

      bind_hostname: "yoda"
      bind_domain_name: "jedi"
      bind_is_caching: yes
      bind_first_forwarder: 8.8.8.8
      bind_second_forwarder: 8.8.4.4
    
## Example

    - hosts: host
      roles:
         - { role: mrLarbi.dns-bind }

## License

GNU General Public License (GPL) V3
