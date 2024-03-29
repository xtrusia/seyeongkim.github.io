---
layout: post
title: OVN 101 when Instance creation
nav_order: 2
reflowMarkdown.preferredLineLength: 60
---

# Prerequisite
ovn based openstack env by using juju.


# Log history

this is time serial order.

## nova-compute
{% highlight shell %}
2023-02-06 03:11:57.679
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Starting instance... _do_build_and_run_instance /usr/lib/python3/dist-packages/nova/compute/manager.py:2223                                                                                                                 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Claim successful on node juju-61fe7a-default-8.cloud.sts                                                                                                                                                                      
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Start building networks asynchronously for instance. _build_resources /usr/lib/python3/dist-packages/nova/compute/manager.py:2587                                                                                           
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Allocating IP information in the background. _allocate_network_async /usr/lib/python3/dist-packages/nova/compute/manager.py:1769                                                                                            
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] allocate_for_instance() allocate_for_instance /usr/lib/python3/dist-packages/nova/network/neutron.py:1039
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Start building block device mappings for instance. _build_resources /usr/lib/python3/dist-packages/nova/compute/manager.py:2622                                                                                             
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Start spawning the instance on the hypervisor. _build_and_run_instance /usr/lib/python3/dist-packages/nova/compute/manager.py:2421                                                                                          
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Creating instance directory _create_image /usr/lib/python3/dist-packages/nova/virt/libvirt/driver.py:3990                                                                                                               
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Creating image
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Created local disks _create_image /usr/lib/python3/dist-packages/nova/virt/libvirt/driver.py:4123 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Ensure instance console log exists: /var/lib/nova/instances/e0bac7d5-917d-4f51-a9bf-a9fa50851876/console.log _ensure_console_log_for_instance /usr/lib/python3/dist-packages/nova/virt/libvirt/driver.py:3878
{% endhighlight %}

## neutron-server
{% highlight shell %}
2023-02-06 03:12:00.338 
Running txn n=1 command(idx=0): AddLSwitchPortCommand(lport=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, lswitch=neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14, may_exist=True, columns={'addresses': ['fa:16:3e:24:08:c3 192.168.21.151'], 'external_ids': {'neutron:port_name': '', 'neutron:device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'neutron:project_id': '627320c412d44ae5a99c1509d5eef68d', 'neutron:cidrs': '192.168.21.151/24', 'neutron:device_owner': '', 'neutron:network_name': 'neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14', 'neutron:security_group_ids': '3f855d03-9cae-4760-9c26-a217f1cc8bbd', 'neutron:revision_number': '1'}, 'parent_name': [], 'tag': [], 'enabled': True, 'options': {'requested-chassis': '', 'mcast_flood_reports': 'true'}, 'type': '', 'port_security': ['fa:16:3e:24:08:c3 192.168.21.151'], 'dhcpv4_options': [UUID('fa8e6a9a-28c1-42f3-b06a-801efe50a448')], 'dhcpv6_options': []}) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                                                                                                                                                                                                                                  
2023-02-06 03:12:00
Running txn n=1 command(idx=1): PgAddPortCommand(port_group=neutron_pg_drop, lsp=[<neutron.plugins.ml2.drivers.ovn.mech_driver.ovsdb.commands.AddLSwitchPortCommand object at 0x7f679d9ba0a0>], if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                   
Running txn n=1 command(idx=2): PgAddPortCommand(port_group=pg_3f855d03_9cae_4760_9c26_a217f1cc8bbd, lsp=[<neutron.plugins.ml2.drivers.ovn.mech_driver.ovsdb.commands.AddLSwitchPortCommand object at 0x7f679d9ba0a0>], if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                           
Running txn n=1 command(idx=3): QoSDelCommand(switch=neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14, conditions=[('direction', '=', 'to-lport'), ('priority', '=', 2002), ('match', '=', 'outport == "0ab6a346-6181-4fa1-81e2-3203cf8a2a66"')]) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                      
Running txn n=1 command(idx=4): QoSDelCommand(switch=neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14, conditions=[('direction', '=', 'from-lport'), ('priority', '=', 2002), ('match', '=', 'inport == "0ab6a346-6181-4fa1-81e2-3203cf8a2a66"')]) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                     
Running txn n=1 command(idx=0): CheckRevisionNumberCommand(name=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, resource={'id': '0ab6a346-6181-4fa1-81e2-3203cf8a2a66', 'name': '', 'network_id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'mac_address': 'fa:16:3e:24:08:c3', 'admin_state_up': True, 'status': 'DOWN', 'device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'device_owner': 'compute:nova', 'fixed_ips': [{'subnet_id': '842cf434-9891-4ba3-8722-ebe727aefe30', 'ip_address': '192.168.21.151'}], 'allowed_address_pairs': [], 'extra_dhcp_opts': [], 'security_groups': ['3f855d03-9cae-4760-9c26-a217f1cc8bbd'], 'description': '', 'binding:vnic_type': 'normal', 'binding:profile': {}, 'binding:host_id': 'juju-61fe7a-default-8.cloud.sts', 'binding:vif_type': 'unbound', 'binding:vif_details': {}, 'port_security_enabled': True, 'dns_name': 'test', 'dns_assignment': [{'ip_address': '192.168.21.151', 'hostname': 'test', 'fqdn': 'test.default.stsstack.qa.1ss.'}], 'dns_domain': '', 'ip_allocation': 'immediate', 'tags': [], 'created_at': '2023-02-06T03:11:59Z', 'updated_at': '2023-02-06T03:12:01Z', 'revision_number': 2, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'network': {'id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'name': 'private', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'admin_state_up': True, 'mtu': 1492, 'status': 'ACTIVE', 'subnets': ['842cf434-9891-4ba3-8722-ebe727aefe30'], 'shared': False, 'availability_zone_hints': [], 'availability_zones': [], 'ipv4_address_scope': None, 'ipv6_address_scope': None, 'router:external': False, 'vlan_transparent': None, 'description': '', 'port_security_enabled': True, 'dns_domain': '', 'l2_adjacency': True, 'tags': [], 'created_at': '2023-02-06T02:01:42Z', 'updated_at': '2023-02-06T02:01:43Z', 'revision_number': 2, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'provider:network_type': 'geneve', 'provider:physical_network': None, 'provider:segmentation_id': 5}}, resource_type=ports, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86
{% endhighlight %}
## nova-compute
{% highlight shell %}
2023-02-06 03:12:01.102
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Successfully created port: 0ab6a346-6181-4fa1-81e2-3203cf8a2a66 _create_port_minimal /usr/lib/python3/dist-packages/nova/network/neutron.py:549 
{% endhighlight %}
## neutron-server
{% highlight shell %}
2023-02-06 03:12:01.761 
Running txn n=1 command(idx=1): SetLSwitchPortCommand(lport=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, columns={'external_ids': {'neutron:port_name': '', 'neutron:device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'neutron:project_id': '627320c412d44ae5a99c1509d5eef68d', 'neutron:cidrs': '192.168.21.151/24', 'neutron:device_owner': 'compute:nova', 'neutron:network_name': 'neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14', 'neutron:security_group_ids': '3f855d03-9cae-4760-9c26-a217f1cc8bbd', 'neutron:revision_number': '2'}, 'parent_name': [], 'tag': [], 'options': {'requested-chassis': 'juju-61fe7a-default-8.cloud.sts', 'mcast_flood_reports': 'true'}, 'enabled': True, 'port_security': ['fa:16:3e:24:08:c3 192.168.21.151'], 'dhcpv4_options': [UUID('fa8e6a9a-28c1-42f3-b06a-801efe50a448')], 'dhcpv6_options': [], 'type': '', 'addresses': ['fa:16:3e:24:08:c3 192.168.21.151'], 'ha_chassis_group': []}, if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                                                                                                                                                                                                                    
Running txn n=1 command(idx=2): PgAddPortCommand(port_group=neutron_pg_drop, lsp=['0ab6a346-6181-4fa1-81e2-3203cf8a2a66'], if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                             
Running txn n=1 command(idx=3): DbRemoveCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=('host-192-168-21-151',), keyvalues={}, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                             
Running txn n=1 command(idx=4): DbRemoveCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=('host-192-168-21-151.default.stsstack.qa.1ss',), keyvalues={}, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                     
Running txn n=1 command(idx=5): DbRemoveCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=('151.21.168.192.in-addr.arpa',), keyvalues={}, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                          
Running txn n=1 command(idx=6): DbAddCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=({'test': '192.168.21.151'},)) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                              
Running txn n=1 command(idx=7): DbAddCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=({'test.default.stsstack.qa.1ss': '192.168.21.151'},)) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                      
Running txn n=1 command(idx=8): DbAddCommand(table=DNS, record=d8dbeaad-6adc-46fc-a7ab-c2ac3cb5c271, column=records, values=({'151.21.168.192.in-addr.arpa': 'test.default.stsstack.qa.1ss'},)) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                         
2023-02-06 03:12:02.683
Running txn n=1 command(idx=0): CheckRevisionNumberCommand(name=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, resource={'id': '0ab6a346-6181-4fa1-81e2-3203cf8a2a66', 'name': '', 'network_id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'mac_address': 'fa:16:3e:24:08:c3', 'admin_state_up': True, 'status': 'DOWN', 'device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'device_owner': 'compute:nova', 'fixed_ips': [{'subnet_id': '842cf434-9891-4ba3-8722-ebe727aefe30', 'ip_address': '192.168.21.151'}], 'allowed_address_pairs': [], 'extra_dhcp_opts': [], 'security_groups': ['3f855d03-9cae-4760-9c26-a217f1cc8bbd'], 'description': '', 'binding:vnic_type': 'normal', 'binding:profile': {}, 'binding:host_id': 'juju-61fe7a-default-8.cloud.sts', 'binding:vif_type': 'ovs', 'binding:vif_details': {'port_filter': True}, 'port_security_enabled': True, 'dns_name': 'test', 'dns_assignment': [{'ip_address': '192.168.21.151', 'hostname': 'test', 'fqdn': 'test.default.stsstack.qa.1ss.'}], 'dns_domain': '', 'ip_allocation': 'immediate', 'tags': [], 'created_at': '2023-02-06T03:11:59Z', 'updated_at': '2023-02-06T03:12:01Z', 'revision_number': 2, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'network': {'id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'name': 'private', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'admin_state_up': True, 'mtu': 1492, 'status': 'ACTIVE', 'subnets': ['842cf434-9891-4ba3-8722-ebe727aefe30'], 'shared': False, 'availability_zone_hints': [], 'availability_zones': [], 'ipv4_address_scope': None, 'ipv6_address_scope': None, 'router:external': False, 'vlan_transparent': None, 'description': '', 'port_security_enabled': True, 'dns_domain': '', 'l2_adjacency': True, 'tags': [], 'created_at': '2023-02-06T02:01:42Z', 'updated_at': '2023-02-06T02:01:43Z', 'revision_number': 2, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'provider:network_type': 'geneve', 'provider:physical_network': None, 'provider:segmentation_id': 5}}, resource_type=ports, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                                                                                                                                           
Running txn n=1 command(idx=1): SetLSwitchPortCommand(lport=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, columns={'external_ids': {'neutron:port_name': '', 'neutron:device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'neutron:project_id': '627320c412d44ae5a99c1509d5eef68d', 'neutron:cidrs': '192.168.21.151/24', 'neutron:device_owner': 'compute:nova', 'neutron:network_name': 'neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14', 'neutron:security_group_ids': '3f855d03-9cae-4760-9c26-a217f1cc8bbd', 'neutron:revision_number': '2'}, 'parent_name': [], 'tag': [], 'options': {'requested-chassis': 'juju-61fe7a-default-8.cloud.sts', 'mcast_flood_reports': 'true'}, 'enabled': True, 'port_security': ['fa:16:3e:24:08:c3 192.168.21.151'], 'dhcpv4_options': [UUID('fa8e6a9a-28c1-42f3-b06a-801efe50a448')], 'dhcpv6_options': [], 'type': '', 'addresses': ['fa:16:3e:24:08:c3 192.168.21.151'], 'ha_chassis_group': []}, if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                                                                                                                                                                                                                    
Running txn n=1 command(idx=2): PgAddPortCommand(port_group=neutron_pg_drop, lsp=['0ab6a346-6181-4fa1-81e2-3203cf8a2a66'], if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86 
{% endhighlight %}
## nova-compute
{% highlight shell %}
2023-02-06 03:12:02.765
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Successfully updated port: 0ab6a346-6181-4fa1-81e2-3203cf8a2a66 _update_port /usr/lib/python3/dist-packages/nova/network/neutron.py:587                                                                                     
2023-02-06 03:12:02.793
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Building network info cache for instance _get_instance_nw_info /usr/lib/python3/dist-packages/nova/network/neutron.py:1873
2023-02-06 03:12:02.881
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Instance cache missing network info. _get_preexisting_port_ids /usr/lib/python3/dist-packages/nova/network/neutron.py:3030 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Updating instance_info_cache with network_info: [{"id": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "address": "fa:16:3e:24:08:c3", "network": {"id": "b34fc699-b789-45a0-a4de-78b8e2c60f14", "bridge": "br-int", "label": "priv
ate", "subnets": [{"cidr": "192.168.21.0/24", "dns": [], "gateway": {"address": "192.168.21.1", "type": "gateway", "version": 4, "meta": {}}, "ips": [{"address": "192.168.21.151", "type": "fixed", "version": 4, "meta": {}, "floating_ips": []}], "routes": [], "version": 4, "meta": {"dhcp_server": "192.168.21.2"}}], "meta": {"injected": false, "tenant_id": "627320c412d44ae5a99c1509d5eef68d", "mtu": 1492, "physical_network": null, "tunneled": true}}, "type": "ovs", "details": {"port_filter": true}, "devname": "tap0ab6a3
46-61", "ovs_interfaceid": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "qbh_params": null, "qbg_params": null, "active": false, "vnic_type": "normal", "profile": {}, "preserve_on_delete": false, "meta": {}}] update_instance_cache_with_nw_info /usr/lib/python3/dist-packages/nova/network/neutron.py:118                                                                                                                                                                                                                                 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Instance network_info: |[{"id": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "address": "fa:16:3e:24:08:c3", "network": {"id": "b34fc699-b789-45a0-a4de-78b8e2c60f14", "bridge": "br-int", "label": "private", "subnets": [{"cidr": "192.168.21.0/24", "dns": [], "gateway": {"address": "192.168.21.1", "type": "gateway", "version": 4, "meta": {}}, "ips": [{"address": "192.168.21.151", "type": "fixed", "version": 4, "meta": {}, "floating_ips": []}], "routes": [], "version": 4, "meta": {"dhcp_server": "192.168.21.2"}}], "meta": {"injected": false, "tenant_id": "627320c412d44ae5a99c1509d5eef68d", "mtu": 1492, "physical_network": null, "tunneled": true}}, "type": "ovs", "details": {"port_filter": true}, "devname": "tap0ab6a346-61", "ovs_interfaceid": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "qbh_params": null, "qbg_params": null, "active": false, "vnic_type": "normal", "profile": {}, "preserve_on_delete": false, "meta": {}}]| _allocate_network_async /usr/lib/python3/dist-packages/nova/compute/manager.py:1783
2023-02-06 03:12:03.350
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Start _get_guest_xml network_info=[{"id": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "address": "fa:16:3e:24:08:c3", "network": {"id": "b34fc699-b789-45a0-a4de-78b8e2c60f14", "bridge": "br-int", "label": "private[0/752]
nets": [{"cidr": "192.168.21.0/24", "dns": [], "gateway": {"address": "192.168.21.1", "type": "gateway", "version": 4, "meta": {}}, "ips": [{"address": "192.168.21.151", "type": "fixed", "version": 4, "meta": {}, "floating_ips": []}], "routes": [], "version": 4, "meta": {"dhcp_server": "192.168.21.2"}}], "meta": {"injected": false, "tenant_id": "627320c412d44ae5a99c1509d5eef68d", "mtu": 1492, "physical_network": null, "tunneled": true}}, "type": "ovs", "details": {"port_filter": true}, "devname": "tap0ab6a346-61", "o
vs_interfaceid": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "qbh_params": null, "qbg_params": null, "active": false, "vnic_type": "normal", "profile": {}, "preserve_on_delete": false, "meta": {}}] disk_info={'disk_bus': 'virtio', 'cdrom_bus': 'ide', 'mapping': {'root': {'bus': 'virtio', 'type': 'disk', 'dev': 'vda', 'boot_index': '1'}, 'disk': {'bus': 'virtio', 'type': 'disk', 'dev': 'vda', 'boot_index': '1'}}} image_meta=ImageMeta(checksum='443b7623e27ecf03dc9e01ee93f67afe',container_format='bare',created_at=2023-02-06
T02:02:43Z,direct_url=<?>,disk_format='qcow2',id=12f634f1-f5eb-4655-8866-ead77b80ed63,min_disk=0,min_ram=0,name='cirros',owner='627320c412d44ae5a99c1509d5eef68d',properties=ImageMetaProps,protected=<?>,size=12716032,status='active',tags=<?>,updated_at=2023-02-06T02:02:43Z,virtual_size=<?>,visibility=<?>) rescue=None block_device_info={'root_device_name': '/dev/vda', 'ephemerals': [], 'block_device_mapping': [], 'swap': None} _get_guest_xml /usr/lib/python3/dist-packages/nova/virt/libvirt/driver.py:6408
{% endhighlight %}
### ovs-vswitchd.log
{% highlight shell %}
2023-02-06T03:12:04.241Z|00060|bridge|INFO|bridge br-int: added interface tap0ab6a346-61 on port 4
{% endhighlight %}
### ovn-controller.log
{% highlight shell %}
2023-02-06T03:12:04.258Z|00023|binding|INFO|Claiming lport 0ab6a346-6181-4fa1-81e2-3203cf8a2a66 for this chassis.
2023-02-06T03:12:04.258Z|00024|binding|INFO|0ab6a346-6181-4fa1-81e2-3203cf8a2a66: Claiming fa:16:3e:24:08:c3 192.168.21.151
{% endhighlight %}
### ovs-vswitchd.log
{% highlight shell %}
2023-02-06T03:12:05.065Z|00061|bridge|INFO|bridge br-int: added interface tapb34fc699-b0 on port 5
{% endhighlight %}
## neutron-server
{% highlight shell %}
Running txn n=1 command(idx=0): CheckRevisionNumberCommand(name=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, resource={'id': '0ab6a346-6181-4fa1-81e2-3203cf8a2a66', 'name': '', 'network_id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'mac_address': 'fa:16:3e:24:08:c3', 'admin_state_up': True, 'status': 'ACTIVE', 'device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'device_owner': 'compute:nova', 'fixed_ips': [{'subnet_id': '842cf434-9891-4ba3-8722-ebe727aefe30', 'ip_address': '192.168.21.151'}], 'allowed_address_pairs': [], 'extra_dhcp_opts': [], 'security_groups': ['3f855d03-9cae-4760-9c26-a217f1cc8bbd'], 'description': '', 'binding:vnic_type': 'normal', 'binding:profile': {}, 'binding:host_id': 'juju-61fe7a-default-8.cloud.sts', 'binding:vif_type': 'ovs', 'binding:vif_details': {'port_filter': True}, 'port_security_enabled': True, 'dns_name': 'test', 'dns_assignment': [{'ip_address': '192.168.21.151', 'hostname': 'test', 'fqdn': 'test.default.stsstack.qa.1ss.'}], 'dns_domain': '', 'ip_allocation': 'immediate', 'tags': [], 'created_at': '2023-02-06T03:11:59Z', 'updated_at': '2023-02-06T03:12:04Z', 'revision_number': 4, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'network': {'id': 'b34fc699-b789-45a0-a4de-78b8e2c60f14', 'name': 'private', 'tenant_id': '627320c412d44ae5a99c1509d5eef68d', 'admin_state_up': True, 'mtu': 1492, 'status': 'ACTIVE', 'subnets': ['842cf434-9891-4ba3-8722-ebe727aefe30'], 'shared': False, 'availability_zone_hints': [], 'availability_zones': [], 'ipv4_address_scope': None, 'ipv6_address_scope': None, 'router:external': False, 'vlan_transparent': None, 'description': '', 'port_security_enabled': True, 'dns_domain': '', 'l2_adjacency': True, 'tags': [], 'created_at': '2023-02-06T02:01:42Z', 'updated_at': '2023-02-06T02:01:43Z', 'revision_number': 2, 'project_id': '627320c412d44ae5a99c1509d5eef68d', 'provider:network_type': 'geneve', 'provider:physical_network': None, 'provider:segmentation_id': 5}}, resource_type=ports, if_exists=True) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                                                                                                                                         
Running txn n=1 command(idx=1): SetLSwitchPortCommand(lport=0ab6a346-6181-4fa1-81e2-3203cf8a2a66, columns={'external_ids': {'neutron:port_name': '', 'neutron:device_id': 'e0bac7d5-917d-4f51-a9bf-a9fa50851876', 'neutron:project_id': '627320c412d44ae5a99c1509d5eef68d', 'neutron:cidrs': '192.168.21.151/24', 'neutron:device_owner': 'compute:nova', 'neutron:network_name': 'neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14', 'neutron:security_group_ids': '3f855d03-9cae-4760-9c26-a217f1cc8bbd', 'neutron:revision_number': '4'}, 'parent_name': [], 'tag': [], 'options': {'requested-chassis': 'juju-61fe7a-default-8.cloud.sts', 'mcast_flood_reports': 'true'}, 'enabled': True, 'port_security': ['fa:16:3e:24:08:c3 192.168.21.151'], 'dhcpv4_options': [UUID('fa8e6a9a-28c1-42f3-b06a-801efe50a448')], 'dhcpv6_options': [], 'type': '', 'addresses': ['fa:16:3e:24:08:c3 192.168.21.151'], 'ha_chassis_group': []}, if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86                                                                                                                                                                                                                                             
Running txn n=1 command(idx=2): PgAddPortCommand(port_group=neutron_pg_drop, lsp=['0ab6a346-6181-4fa1-81e2-3203cf8a2a66'], if_exists=False) do_commit /usr/lib/python3/dist-packages/ovsdbapp/backend/ovs_idl/transaction.py:86
{% endhighlight %}

## nova-compute
{% highlight shell %}
2023-02-06 03:12:05.643
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Received event network-changed-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 external_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:10341                                                                    
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Refreshing instance network info cache due to event network-changed-0ab6a346-6181-4fa1-81e2-3203cf8a2a66. external_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:10346
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Refreshing network info cache for port 0ab6a346-6181-4fa1-81e2-3203cf8a2a66 _get_instance_nw_info /usr/lib/python3/dist-packages/nova/network/neutron.py:1870
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] End _get_guest_xml xml=<domain type="kvm">                                                                                                                                                                              
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Preparing to wait for external event network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 prepare_for_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:320
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Updated VIF entry in instance network info cache for port 0ab6a346-6181-4fa1-81e2-3203cf8a2a66. _build_network_info_model /usr/lib/python3/dist-packages/nova/network/neutron.py:3162
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Updating instance_info_cache with network_info: [{"id": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "address": "fa:16:3e:24:08:c3", "network": {"id": "b34fc699-b789-45a0-a4de-78b8e2c60f14", "bridge": "br-int", "label": "priv
ate", "subnets": [{"cidr": "192.168.21.0/24", "dns": [], "gateway": {"address": "192.168.21.1", "type": "gateway", "version": 4, "meta": {}}, "ips": [{"address": "192.168.21.151", "type": "fixed", "version": 4, "meta": {}, "floating_ips": []}], "routes": [], "version": 4, "meta": {"dhcp_server": "192.168.21.2"}}], "meta": {"injected": false, "tenant_id": "627320c412d44ae5a99c1509d5eef68d", "mtu": 1492, "physical_network": null, "tunneled": true}}, "type": "ovs", "details": {"port_filter": true}, "devname": "tap0ab6a3
46-61", "ovs_interfaceid": "0ab6a346-6181-4fa1-81e2-3203cf8a2a66", "qbh_params": null, "qbg_params": null, "active": false, "vnic_type": "normal", "profile": {}, "preserve_on_delete": false, "meta": {}}] update_instance_cache_with_nw_info /usr/lib/python3/dist-packages/nova/network/neutron.py:118                                                                                                                                                                                                                                 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] VM Started (Lifecycle Event)                                                        
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Checking state _get_power_state /usr/lib/python3/dist-packages/nova/compute/manager.py:1551                                                                                                                                                                                                                                                             
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] VM Paused (Lifecycle Event)                                                         
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Checking state _get_power_state /usr/lib/python3/dist-packages/nova/compute/manager.py:1551                                                                                                                                                                                                                                                             
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Synchronizing instance power state after lifecycle event "Paused"; current vm_state: building, current task_state: spawning, current DB power_state: 0, VM power_state: 3 handle_lifecycle_event /usr/lib/python3/dist-packages/nova/compute/manager.py:1272                                                                                            
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Received event network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 external_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:10341
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Processing event network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 _process_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:10107
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Guest created on hypervisor spawn /usr/lib/python3/dist-packages/nova/virt/libvirt/driver.py:3691                                                                                                                       
./nova-compute.log:2023-02-06 03:12:05.652 56726 INFO nova.virt.libvirt.driver [-] [instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Instance spawned successfully.                                                                                                   
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Took 6.29 seconds to spawn the instance on the hypervisor.                                                                                                                                                                   
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Checking state _get_power_state /usr/lib/python3/dist-packages/nova/compute/manager.py:1551                                                                                                                                 
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] During sync_power_state the instance has a pending task (spawning). Skip.                                                                                                                                                                                                                                                                                
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] VM Resumed (Lifecycle Event)                                                        
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Checking state _get_power_state /usr/lib/python3/dist-packages/nova/compute/manager.py:1551                                                                                                                                                                                                                                                             
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Synchronizing instance power state after lifecycle event "Resumed"; current vm_state: building, current task_state: spawning, current DB power_state: 0, VM power_state: 1 handle_lifecycle_event /usr/lib/python3/dist-packages/nova/compute/manager.py:1272                                                                                           
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] During sync_power_state the instance has a pending task (spawning). Skip.                                                                                                                                                                                                                                                                                
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Received event network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 external_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:10341
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] No waiting events found dispatching network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 pop_instance_event /usr/lib/python3/dist-packages/nova/compute/manager.py:357
[instance: e0bac7d5-917d-4f51-a9bf-a9fa50851876] Received unexpected event network-vif-plugged-0ab6a346-6181-4fa1-81e2-3203cf8a2a66 for instance with vm_state building and task_state spawning.     

{% endhighlight %}

# Status check by using command

{% highlight shell %}
compute ovs-vsctl show
f3af6eb3-a68a-468d-b9a5-645c49e04177
    Manager "ptcp:6640:127.0.0.1"
        is_connected: true
    Bridge br-data
        fail_mode: standalone
        datapath_type: system
        Port patch-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124-to-br-int
            Interface patch-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124-to-br-int
                type: patch
                options: {peer=patch-br-int-to-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124}
        Port br-data
            Interface br-data
                type: internal
        Port ens8
            Interface ens8
                type: system
    Bridge br-int
        fail_mode: secure
        datapath_type: system
        Port tapb34fc699-b0
            Interface tapb34fc699-b0
        Port tap0ab6a346-61
            Interface tap0ab6a346-61
        Port patch-br-int-to-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124
            Interface patch-br-int-to-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124
                type: patch
                options: {peer=patch-provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124-to-br-int}
        Port br-int
            Interface br-int
                type: internal
    ovs_version: "2.13.8"
{% endhighlight %}

{% highlight shell %}
ovn-chassis ovn-nbctl show
switch 5e29b631-a77a-44b7-85c2-e1acde3784ae (neutron-b34fc699-b789-45a0-a4de-78b8e2c60f14) (aka private)
    port 07d48fc4-736f-43fb-a34f-86e64bbc978f
        type: localport
        addresses: ["fa:16:3e:70:89:70 192.168.21.2"]
    port 0ab6a346-6181-4fa1-81e2-3203cf8a2a66
        addresses: ["fa:16:3e:24:08:c3 192.168.21.151"]
    port 8c402a23-89c8-4386-a3ab-6edfb5606206
        type: router
        router-port: lrp-8c402a23-89c8-4386-a3ab-6edfb5606206
switch 3f61a2c3-ea62-48b3-b966-38f34bb177c9 (neutron-21a6e1de-695d-4b76-844e-1c2bce717320) (aka ext_net)
    port bc51714a-8fce-46a5-ba22-e96f8d2db30e
        type: localport
        addresses: ["fa:16:3e:01:0d:01"]
    port 21b6f198-9f52-4dd5-b737-9f38a17ed97c
        type: router
        router-port: lrp-21b6f198-9f52-4dd5-b737-9f38a17ed97c
    port provnet-67d070c2-c2ae-43b0-896f-ae83f1a1f124
        type: localnet
        addresses: ["unknown"]
router ccfe0567-189f-446d-a6ff-3ce9680db35c (neutron-2ac4e6d8-7aab-4932-ad3a-b667ed737e0a) (aka provider-router)
    port lrp-21b6f198-9f52-4dd5-b737-9f38a17ed97c
        mac: "fa:16:3e:8e:d5:1b"
        networks: ["10.5.153.188/16"]
        gateway chassis: [juju-61fe7a-default-8.cloud.sts]
    port lrp-8c402a23-89c8-4386-a3ab-6edfb5606206
        mac: "fa:16:3e:4f:71:dc"
        networks: ["192.168.21.1/24"]
    nat 79dbbed7-3697-417b-a828-45d4e71dcafb
        external ip: "10.5.153.188"
        logical ip: "192.168.21.0/24"
        type: "snat"
{% endhighlight %}

{% highlight shell %}
ovn-chassis ovn-sbctl show
Chassis juju-61fe7a-default-8.cloud.sts
    hostname: juju-61fe7a-default-8.cloud.sts
    Encap geneve
        ip: "10.5.2.34"
        options: {csum="true"}
    Port_Binding "0ab6a346-6181-4fa1-81e2-3203cf8a2a66"
    Port_Binding cr-lrp-21b6f198-9f52-4dd5-b737-9f38a17ed97c
{% endhighlight %}