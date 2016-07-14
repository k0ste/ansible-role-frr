ansible-role-quagga
==========================

Role for deploy '[quagga upstream](http://download.savannah.gnu.org/releases/quagga/)' or '[quagga CumulusLinux](//github.com/CumulusNetworks/quagga)'.

Ansible versions
--------------------
Role is adapted for Ansible 2.0.

Requirements for usage
-----------------------------------

* GNU/Linux;
* Quagga upstream or Quagga with CumulusLinux patch set;
* [python-netaddr](//docs.ansible.com/ansible/playbooks_filters_ipaddr.html)
library (on machine with Ansible);

About
----------

* Support zebra daemon;
* Support ospfd daemon;
* ospfd multi instance (CumulusLinux);
* Access lists;
* Prefix lists;
* Route maps;
* Static routes;
* Interfaces.

Extra
-----------

You can enable/disable quagga management by this role via vars:

* **quagga_zebra_mgmt**, *default is true*
* **quagga_ospfd_mgmt**, *default is true*

Behavior of handlers rule by variables:

* **quagga_enable**, *default is false*
* **quagga_restart**, *default is false*

**quagga_cumulus**, if true - enable CumulusLinux OSPF multi instance support.
**quagga_ospfd_instances** - this is list with your ospfd instances, example:

```yaml
---
quagga_ospfd_instances:
  - 126
  - 17

quagga_router_ospf_multi:
- instance_id: '126'
  router_id: '172.16.255.1'
- instance_id: '17'
  router_id: '192.168.1.1'
```

This is need, because Ansible can't access to dict, for work with_items.

Example configuration
-------------------------

```yaml
---
quagga_restart: 'true'
quagga_enable: 'true'
quagga_zebra_mgmt: 'true'
quagga_ospfd_mgmt: 'true'
quagga_cumulus: 'true'
quagga_ospfd_instances:
  - 1
  - 2

quagga_zebra_conft:
- hostname: 'R1'
  password: 'passw'
  enable_password: 'en_passwd'
  ip_forwarding: 'true'
  service:
  - password_encryption: 'true'
    integrated_vtysh: 'false'
    advanced_vty: 'true'
  log:
  - stdout: 'informational'
    monitor: 'informational'
    syslog: 'informational'
    facility: 'local7'
  debug:
  - events: 'true'
    fpm: 'false'
    kernel: 'true'
    nht: 'false'
    packet:
    - state: 'false'
      detail: 'true'
    rib:
    - state: 'false'
      detail: 'true'
  line:
    - vty_access_class: 'vty'

quagga_static_route:
- destination: '0.0.0.0/0'
  next_hop: '5.128.220.1'
  distance: '2'
- destination: '0.0.0.0/0'
  next_hop: '5.128.220.2'
  distance: '3'
- destination: '10.0.0.1/32'
  next_hop: 'reject'
  tag: '222'

quagga_access_list:
- name: 'vty'
  remark: 'Disable connections to vtysh from non localhost'
  rules:
  - "permit 127.0.0.1/8"
  - "deny any"
- name: '101'
  remark: 'Filter 0.0.0.0/0'
  rules:
  - "deny ip host 0.0.0.0 any"
  - "permit ip any any"

quagga_prefix_list:
- name: 'EXAMPLE_PREFIX_LIST'
  description: 'Opentech OSPF'
  rules:
  - 'seq 10 permit 192.168.0.0/16 le 32'
  - 'seq 100 deny any'
- name: 'no_default_originate'
  description: 'Filter gateway of last resort'
  rules:
  - "seq 10 deny 0.0.0.0/0"
  - "seq 100 permit 0.0.0.0/0 le 32"

quagga_route_map:
- name: 'DISTRIBUTE_TO_OSPF'
  actions:
  - action: 'permit 10'
    rules:
    - 'match ip address prefix-list opentech_ospf'
  - action: 'deny 100'
- name: 'TO_OSPF_NTK'
  actions:
  - action: 'permit 10'
    rules:
    - 'match ip address prefix-list EXAMPLE_PREFIX_LISTf'
    - 'match ip next-hop prefix-list i_give_up'
  - action: 'permit 11'
    rules:
    - 'match ip address prefix-list FROM_TO'
  - action: 'deny 100'

quagga_interfaces:
- name: 'lo'
  description: 'Loopback0'
  link_detect: 'true'
  multicast: 'true'
  ip:
  - address: '10.0.0.1/8'
    label: 'EXAMPLE_LABEL_FOR_IPROUTE'
- name: 'tap0'
  description: 'openvpn0 description'
  ip:
  - ospf:
    - cost: '100'
    - instance_id: '1'
      area: '0.0.0.2'
    - instance_id: '2'
      area: '0.0.0.3'
- name: 'tap1'
  description: 'openvpn1 description'
  ip:
  - ospf:
    - cost: '10'
    - instance_id: '1'
      area: '0.0.0.3'
    - instance_id: '2'
      area: '0.0.0.3'
- name: 'tap2'
  description: 'openvpn2 description'
  ip:
  - ospf:
      - cost: '10'

quagga_router_ospf:
- router_id: '192.168.1.1'
  log_adjacency_changes: 'detail'
  default_information_originate:
    - enabled: 'always'
      route_map: 'DEFAULT_INFORMATION'
      metric: '40'
      metric_type: '2'
  passive_interface:
    - 'Tunnel0'
    - 'Tunnel1'
  area:
  - area_id: '0.0.0.1'
    filter_list_in: 'FILTER_PREFIX_IN'
    filter_list_out: 'FILTER_PREFIX_OUT'
    import_list: 'ACCESS_LIST_IMPORT'
    export_list: 'ACCESS_LIST_EXPORT'
  network:
  - network: '192.168.0.0/16'
    area: '0.0.0.1'
  - network: '172.16.255.0/24'
    area: '0.0.0.2'
  redistribute:
  - connected:
    - route_map: 'TO_OSPF_CONNECTED'
      metric: '27'
      metric_type: '1'
  - static:
    - route_map: 'TO_OSPF_STATIC'
      metric: '28'
      metric_type: '2'
  - bgp:
    - route_map: 'TO_OSPF_BGP'
      metric: '45'
      metric_type: '2'
  - kernel:
    - route_map: 'TO_OSPF_KERNEL'
      metric: '46'
      metric_type: '2'
  - ospf:
    - instance_id: '1'
      route_map: 'FROM_OSPF_1_TO_OSPF_2'

quagga_router_ospf_multi:
- instance_id: '1'
  router_id: '172.16.255.1'
  log_adjacency_changes: 'true'
  default_information_originate:
    - enabled: 'true'
  redistribute:
  - connected:
    - route_map: 'TO_OSPF_CONNECTED'
  - static:
    - route_map: 'TO_OSPF_STATIC'
  - bgp:
    - route_map: 'TO_OSPF_BGP'
  - kernel:
    - route_map: 'TO_OSPF_KERNEL'
  - ospf:
    - instance_id: '2'
- instance_id: '2'
  router_id: '192.168.1.1'
  log_adjacency_changes: 'detail'
  default_information_originate:
    - enabled: 'always'
      route_map: 'DEFAULT_INFORMATION'
      metric: '40'
      metric_type: '2'
  passive_interface:
    - 'Tunnel0'
    - 'Tunnel1'
  area:
  - area_id: '0.0.0.1'
    filter_list_in: 'FILTER_PREFIX_IN'
    filter_list_out: 'FILTER_PREFIX_OUT'
    import_list: 'ACCESS_LIST_IMPORT'
    export_list: 'ACCESS_LIST_EXPORT'
  redistribute:
  - connected:
    - route_map: 'TO_OSPF_CONNECTED'
      metric: '27'
      metric_type: '1'
  - static:
    - route_map: 'TO_OSPF_STATIC'
      metric: '28'
      metric_type: '2'
  - bgp:
    - route_map: 'TO_OSPF_BGP'
      metric: '45'
      metric_type: '2'
  - kernel:
    - route_map: 'TO_OSPF_KERNEL'
      metric: '46'
      metric_type: '2'
  - ospf:
    - instance_id: '1'
      route_map: 'FROM_OSPF_1_TO_OSPF_2'
```
