# ansible-role-frr

Role for deploy [Free Range Routing](//frrouting.org/) package and router
configuration.

## Requirements for usage

* Ansible 3.0.0+;

## Example configuration

```yaml
---
frr:
# Enable frr service or not.
- enable: 'true'
# Restart frr service after deploy or not. Actually restart is not performed,
# only 'reload' (merge current configuration in RAM with 'frr.conf').
  restart: 'true'
# Install frr package or not.
  install_package: 'true'
  daemons:
  - bgpd: 'yes'
    ospfd: 'yes'
    ospf6d: 'no'
    ripd: 'no'
    ripngd: 'no'
    isisd: 'no'
    pimd: 'no'
    ldpd: 'no'
    nhrpd: 'no'
    eigrpd: 'no'
    babeld: 'no'
    sharpd: 'no'
    pbrd: 'yes'
    bfdd: 'yes'
    fabricd: 'no'
    vrrpd: 'yes'
    zebra: 'yes'
  vtysh_options:
  - vtysh_enable: 'yes'
    zebra_options: '-A 127.0.0.1 -s 90000000 -d'
    bgpd_options: '-A 127.0.0.1 -M rpki -d'
    ospfd_options: '-A 127.0.0.1 -d'
    ospf6d_options: '-A ::1 -d'
    ripd_options: '-A 127.0.0.1 -d'
    ripngd_options: '-A ::1 -d'
    isisd_options: '-A 127.0.0.1 -d'
    pimd_options: '-A 127.0.0.1 -d'
    ldpd_options: '-A 127.0.0.1 -d'
    nhrpd_options: '-A 127.0.0.1 -d'
    eigrpd_options: '-A 127.0.0.1 -d'
    babeld_options: '-A 127.0.0.1 -d'
    sharpd_options: '-A 127.0.0.1 -d'
    pbrd_options: '-A 127.0.0.1 -d'
    staticd_options: '-A 127.0.0.1 -d'
    bfdd_options: '-A 127.0.0.1 -d'
    fabricd_options: '-A 127.0.0.1 -d'
    vrrpd_options: '-A 127.0.0.1 -d'
  init_options:
  - max_fds: '1024'
    watchfrr_enable: 'yes'
    watchfrr_options: "-d -r '/usr/bin/frr restart %s' -s '/usr/bin/frr start %s' -k '/usr/bin/frr stop %s'"
  settings:
  - zebra:
    - hostname: 'R1'
      log_commands: 'true'
      log_facility: 'local7'
      log_file: 'vtysh.log'
      log_monitor: 'informational'
      log_record_priority: 'true'
      log_stdout: 'informational'
      log_syslog: 'informational'
      # from '0' to '6'
      log_timestamp_precision: '1'
      service_integrated_vtysh_config: 'true'
      service_advanced_vty: 'true'
      service_password_encryption: 'true'
# debug: 'all', 'common', 'filter', 'interface', 'kernel', 'route', 'timeout'
      debug_babel: 'all'
      debug_bgp_allow_martians: 'true'
      debug_bgp_as4_segment: 'true'
# Enable or disable debugging for bestpath selection on the specified prefix.
      debug_bgp_bestpath:
      - '100.100.100.0/24'
      - '100.100.101.0/24'
# Enable or disable debugging for BGP keepalives. This provides information on
# BGP KEEPALIVE messages transmitted and received between local and remote
# instances.
      debug_bgp_keepalives: 'true'
      debug_bgp_keepalives_peers:
      - '100.100.100.0/24'
      - '100.100.101.0/24'
      debug_bgp_labelpool: 'true'
# Enable or disable debugging for neighbor events. This provides general
# information on BGP events such as peer connection/disconnection, session
# establishment/teardown, and capability negotiation.
      debug_bgp_neighbor_events: 'true'
      debug_bgp_neighbor_events_peers:
      - '100.100.100.0/24'
      - '100.100.101.0/24'
# Enable or disable debugging of BGP nexthop tracking.
      debug_bgp_nht: 'true'
      debug_bgp_pbr: 'true'
      debug_bgp_pbr_error: 'true'
# Enable or disable debugging of dynamic update groups. This provides general
# information on group creation, deletion, join and prune events.
      debug_bgp_update_groups: 'true'
# Enable or disable debugging for BGP updates. This provides information on
# BGP UPDATE messages transmitted and received between local and remote
# instances.
      debug_bgp_updates: 'true'
# Enable or disable debugging of communications between bgpd and zebra.
      debug_bgp_zebra: 'true'
      debug_bgp_zebra_prefix:
      - '100.100.100.0/24'
      - '100.100.101.0/24'
      debug_memstats_at_exit: 'true'
# Show debug information of OSPF event.
      debug_ospf_event: 'true'
# Show debug information of Interface State Machine.
      debug_ospf_ism: 'true'
# Show debug information of Network State Machine.
      debug_ospf_nsm: 'true'
# Show debug information about Not So Stub Area.
      debug_ospf_nssa: 'true'
      debug_ospf_sr: 'true'
# Show debug information about Traffic Engineering LSA.
      debug_ospf_te: 'true'
# Show debug information of ZEBRA API.
      debug_ospf_zebra: 'true'
      debug_pbr_events: 'true'
      debug_pbr_map: 'true'
      debug_pbr_nht: 'true'
      debug_pbr_zebra: 'true'
      debug_route_map: 'true'
# Enable or disable debugging output for RPKI.
      debug_rpki: 'true'
      debug_vrf: 'true'
# Toggle debugging logs for all VRRP components.
      debug_vrrp: 'true'
# VRRP logs actions taken by the ARP component of VRRP.
      debug_vrrp_arp: 'true'
# VRRP logs actions taken by the autoconfiguration procedures.
      debug_vrrp_autoconfigure: 'true'
# VRRP logs actions taken by the Neighbor Discovery component of VRRP.
      debug_vrrp_ndisc: 'true'
# VRRP logs details of ingress and egress packets. Includes packet decodes and
# hex dumps.
      debug_vrrp_packets: 'true'
# VRRP logs state changes, election protocol decisions, and interface status
# changes.
      debug_vrrp_protocol: 'true'
# VRRP logs details of socket configuration and initialization.
      debug_vrrp_sockets: 'true'
# VRRP logs communications with Zebra.
      debug_vrrp_zebra: 'true'
      debug_zebra_dplane: 'true'
      debug_zebra_events: 'true'
      debug_zebra_fpm: 'true'
      debug_zebra_kernel: 'true'
      debug_zebra_mpls: 'true'
      debug_zebra_nht: 'true'
      debug_zebra_packet: 'true'
      debug_zebra_pseudowires: 'true'
      debug_zebra_rib: 'true'
      debug_zebra_vxlan: 'true'
      password: 'this'
      enable_password: 'that'
    vty:
    - access_class: 'this'
      ipv6_access_class: 'that'
    routes:
    - destination: '100.100.100.0/24'
      next_hop: '100.100.100.1'
      distance: '2'
      interface: 'eth0'
    - destination: '0.0.0.0/0'
      next_hop: '5.128.220.1'
      distance: '2'
    - destination: '0.0.0.0/0'
      next_hop: '5.128.220.2'
      distance: '3'
    - destination: '10.0.0.1/32'
      next_hop: 'reject'
      tag: '222'
    vrrp:
    - vrrp_autoconfigure_version: '3'
      default:
      - priority: '243'
        advertisement_interval: '1500'
        shutdown: 'true'
        preempt: 'false'
    interfaces:
    - name: 'lo'
      description: 'Loopback0'
      link_detect: 'true'
      multicast: 'true'
      ip:
      - address: '172.16.254.1/32'
        label: 'Router1'
      - address: '172.16.255.1/32'
        label: 'Router2'
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
    - name: 'vrrp0'
      description: 'VRRP0 neighbor'
      ip:
      - address: '172.16.222.1/30'
      vrrp:
      - router_id: '5'
        ip: '172.16.222.1/30'
        ipv6: ''
        advertisement_interval: '1500'
        priority: '254'
    - name: 'vrrp1'
      description: 'VRRP1 neighbor'
      vrrp:
      - router_id: '5'
        ip: '172.16.223.1/30'
        priority: '250'
        preempt: 'false'
        shutdown: 'true'
    router:
    - ospf:
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
      bgp:
      - as_num: '198181'
        router_id: '100.100.100.1'
        bgp_graceful_restart: 'true'
        neighbors:
        - neighbor: '212.17.15.169'
          remote_as: '25549'
          bfd_check_control_plane_failure: 'true'
          bfd_profile: 'BFD_PROFILE'
          capability: 'dynamic'
          description: 'avantel'
          disable_connected_check: 'true'
          ebgp_multihop: '255'
          enforce_first_as: 'true'
          enforce_multihop: 'true'
          graceful_restart: 'true'
          graceful_restart_disable: 'true'
          graceful_restart_helper: 'true'
          interface: 'vlan597'
          maximum_prefix_out: '220'
          override_capability: 'true'
          passive: 'false'
          peer_group: 'PEER_GROUP_NAME'
          sender_as_path_loop_detection: 'true'
        - neighbor: '95.156.85.193'
          remote_as: '12389'
          description: 'rostelecom'
          solo: 'true'
          tcp_mss: '1400'
        - neighbor: '178.49.129.89'
          remote_as: '31200'
          description: 'novotelecom'
          strict_capability_match: 'true'
        address_family_ipv4_unicast:
        - networks:
          - '193.150.124.0/24'
          - '193.150.125.0/24'
          neighbors:
          - neighbor: '212.17.15.169'
            distribute_list:
            - in: '101'
            prefix_list:
            - out: 'opentech_bgp_advertise'
            route_map:
            - in: 'opentech_avantel_in'
              out: 'opentech_avantel_out'
            soft_reconfiguration_inbound: 'true'
          - neighbor: '95.156.85.193'
            distribute_list:
            - in: '101'
            prefix_list:
            - out: 'opentech_bgp_advertise'
            route_map:
            - in: 'opentech_rostelecom_in'
              out: 'opentech_rostelecom_out'
            soft_reconfiguration_inbound: 'true'
          - neighbor: '178.49.129.89'
            distribute_list:
            - in: '101'
            prefix_list:
            - out: 'opentech_bgp_advertise'
            route_map:
            - in: 'opentech_novotelecom_in'
              out: 'opentech_novotelecom_out'
            soft_reconfiguration_inbound: 'true'
      bgp_as_path_access_list:
      - name: 'avantel'
        rules:
        - 'permit ^_25549$'
      - name: 'ertelecom'
        rules:
        - 'permit ^_9049$'
      - name: 'ixonly_in'
        rules:
        - 'permit ^_[0-9]*$'
      - name: 'novotelecom'
        rules:
        - 'permit ^_31200$'
      - name: 'rostelecom'
        rules:
        - 'permit ^_12389$'
      route_map:
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
          - 'match ip address prefix-list EXAMPLE_PREFIX_LIST'
          - 'match ip next-hop prefix-list i_give_up'
        - action: 'permit 11'
          rules:
          - 'match ip address prefix-list FROM_TO'
        - action: 'deny 100'
      - name: 'opentech_avantel_in'
        actions:
        - action: 'permit 10'
          rules:
          - 'match as-path avantel'
          - 'set local-preference 300'
        - action: 'permit 20'
          rules:
          - 'set local-preference 100'
      - name: 'opentech_avantel_out'
        actions:
        - action: 'permit 10'
      - name: 'opentech_rostelecom_in'
        actions:
        - action: 'permit 10'
          rules:
          - 'match as-path rostelecom'
          - 'set local-preference 300'
        - action: 'permit 20'
          rules:
          - 'set local-preference 100'
      - name: 'opentech_rostelecom_out'
        actions:
        - action: 'permit 10'
      - name: 'opentech_novotelecom_in'
        actions:
        - action: 'permit 10'
          rules:
          - 'match as-path novotelecom'
          - 'set local-preference 300'
        - action: 'permit 20'
          rules:
          - 'set local-preference 100'
      - name: 'opentech_novotelecom_out'
        actions:
        - action: 'permit 10'
      - name: 'blackhole'
        actions:
        - action: 'permit 10'
          rules:
          - 'match ip address prefix-list blackhole'
          - 'set community 12389:55555'
      access_list:
      - name: 'vty'
        remark: 'Disable connections to vtysh from non localhost'
        rules:
        - 'permit 127.0.0.1/8'
        - 'deny any'
      - name: '101'
        remark: 'Filter 0.0.0.0/0'
        rules:
        - 'deny ip host 0.0.0.0 any'
        - 'permit ip any any'
      prefix_list:
      - name: 'EXAMPLE_PREFIX_LIST'
        description: 'Opentech OSPF'
        rules:
        - 'permit 192.168.0.0/16 le 32'
        - 'deny any'
      - name: 'no_default_originate'
        description: 'Filter gateway of last resort'
        rules:
        - 'deny 0.0.0.0/0'
        - 'permit 0.0.0.0/0 le 32'
      - name: 'blackhole'
        description: 'BlackHole RFC 7999'
        rules:
        - 'permit 193.150.124.100/32'
        - 'deny any'
      - name: 'opentech_bgp_advertise'
        description: 'AS198181 ipv4 prefixes'
        rules:
        - 'permit 193.150.124.0/24'
        - 'deny any'
```
