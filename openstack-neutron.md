# Neutron

# ML2 (Modular Layer 2)

## How ML2 is designed
![Neutron ML2 architecture](http://aqorn.com/wp-content/uploads/2014/11/ml2-arch1.png)

### Description
    It currently works with the existing openvswitch, linuxbridge, and hyperv L2 agents, and is intended to replace and deprecate the monolithic plugins associated with those L2 agents. The ml2 framework is also intended to greatly simplify adding support for new L2 networking technologies, requiring much less initial and ongoing effort than would be required to add a new monolithic core plugin.

## ML2 Type drivers
TypeDrivers maintain any needed type-specific network state, and perform provider network validation and tenant network allocation
- Flat
- Local
- VLAN
- GRE
- VXLAN

## ML2 Mechanisms
The MechanismDriver is responsible for taking the information established by the TypeDriver and ensuring that it is properly applied given the specific networking mechanisms that have been enabled
- Linux bridges
- OpenVSwitch
- Cisco Nexus...

## Network type definitions
A **local** network is a network that can only be realized on a single host. This is only used in proof-of-concept or development environments, because just about any other OpenStack environment will have multiple compute hosts and/or a separate network host.

A **flat** network is a network that does not provide any segmentation options. A traditional L2 ethernet network is a "flat" network. Any servers attached to this network are able to see the same broadcast traffic and can contact each other without requiring a router. flat networks are often used to attach Nova servers to an existing L2 network (this is called a "provider network").

A **VLAN** network is one that uses VLANs for segmentation. When you create a new network in Neutron, it will be assigned a VLAN ID from the range you have configured in your Neutron configuration. Using vlan networks requires that any switches in your environment are configured to trunk the corresponding VLANs.

**GRE and VXLAN** networks are very similar. They are both "overylay" networks that work by encapsulating network traffic. Like vlan networks, each network you create receives a unique tunnel id. Unlike vlan networks, an overlay network does not require that you synchronize your OpenStack configuration with your L2 switch configuration.

# L3 agent
Neutron has an API extension to allow administrators and tenants to create "routers" that connect to L2 networks. Known as the "neutron-l3-agent", it uses the Linux IP stack and iptables to perform L3 forwarding and NAT. In order to support multiple routers with potentially overlapping IP addresses, neutron-l3-agent defaults to using Linux network namespaces to provide isolated forwarding contexts.

# Neutron networking node active services
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)
- L3 agent i.e. qrouter (iptables) with different namespaces per tenant/project
- DHCP agent
- Metadata agent

**Note**

VXLAN or GRE are technics used to encapsulate L2 traffic inside an L3 network to bypass VLAN limitations between the compute(s) and the networking node(s).
****

*Networking node overview with legacy Linux br*

![Networking node overview with legacy Linux br](http://docs.openstack.org/networking-guide/_images/scenario-legacy-lb-network1.png)

****

*Networking node overview with new OpenVSwitch*

![Networking node overview with new OpenVSwitch](http://docs.openstack.org/networking-guide/_images/scenario-legacy-ovs-network1.png)

****

*Networking node overview with new OpenVSwitch and GRE encapsulation*

![Networking node overview with new OpenVSwitch and GRE](http://docs.openstack.org/kilo/install-guide/install/yum/content/figures/3/a/common/figures/installguide-neutron-initialnetworks.png)

****

# Neutron controller node active services
- Neutron server service
- ML2 plug-in

# Neutron compute node active services
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)

# Neutron setup without HA

## Neutron on **controller** node

*Common options are into /etc/neutron/neutron.conf*

      [DEFAULT]
      verbose = True
      core_plugin = ml2
      service_plugins = router
      allow_overlapping_ips = True

***

*The ML2 plug-in configuration resides into /etc/neutron/plugins/ml2/ml2_conf.ini*

**Legacy OpenvSwitch**

      [ml2]
      type_drivers = flat,vlan,gre,vxlan
      tenant_network_types = vlan,gre,vxlan
      mechanism_drivers = openvswitch,l2population

      [ml2_type_flat]
      flat_networks = external

      [ml2_type_vlan]
      network_vlan_ranges = external,vlan:MIN_VLAN_ID:MAX_VLAN_ID

      [ml2_type_gre]
      tunnel_id_ranges = MIN_GRE_ID:MAX_GRE_ID

      [ml2_type_vxlan]
      vni_ranges = MIN_VXLAN_ID:MAX_VXLAN_ID
      vxlan_group = 239.1.1.1

      [securitygroup]
      firewall_driver = neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver
      enable_security_group = True
      enable_ipset = True

**Legacy Linux br**

      [ml2]
      type_drivers = flat,vlan,vxlan
      tenant_network_types = vlan,vxlan
      mechanism_drivers = linuxbridge,l2population

      [ml2_type_flat]
      flat_networks = external

      [ml2_type_vlan]
      network_vlan_ranges = external,vlan:MIN_VLAN_ID:MAX_VLAN_ID

      [ml2_type_vxlan]
      vni_ranges = MIN_VXLAN_ID:MAX_VXLAN_ID
      vxlan_group = 239.1.1.1

      [securitygroup]
      firewall_driver = neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
      enable_security_group = True
      enable_ipset = True

***

*Start the service*

      Neutron Server

**Note**

The first value in the tenant_network_types option becomes the default project network type when a non-privileged user creates a network.
The external value in the network_vlan_ranges option lacks VLAN ID ranges to support use of arbitrary VLAN IDs by privileged users.

## Neutron **networking** node

*As usual*

      net.ipv4.ip_forward=1
      net.ipv4.conf.default.rp_filter=0
      net.ipv4.conf.all.rp_filter=0

***

*Edit the ML2 plug-in configuration /etc/neutron/plugins/ml2/ml2_conf.ini*

**Legacy OpenvSwitch**

      [ovs]
      local_ip = TUNNEL_INTERFACE_IP_ADDRESS
      enable_tunneling = True
      bridge_mappings = vlan:br-vlan,external:br-ex

      [agent]
      l2_population = True
      tunnel_types = gre,vxlan

      [securitygroup]
      firewall_driver = neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver
      enable_security_group = True
      enable_ipset = True

**Legacy Linux br**

      [securitygroup]
      firewall_driver = neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
      enable_security_group = True
      enable_ipset = True

      [linux_bridge]
      physical_interface_mappings = vlan:PROJECT_VLAN_INTERFACE,external:EXTERNAL_INTERFACE

      [vxlan]
      enable_vxlan = True
      local_ip = TUNNEL_INTERFACE_IP_ADDRESS
      l2_population = True

***

*Configure the L3 Agent here /etc/neutron/l3_agent.ini*

**Legacy OpenvSwitch**

      [DEFAULT]
      verbose = True
      interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
      use_namespaces = True
      external_network_bridge =
      router_delete_namespaces = True

**Legacy Linux br**

      [DEFAULT]
      verbose = True
      interface_driver = neutron.agent.linux.interface.BridgeInterfaceDriver
      use_namespaces = True
      external_network_bridge =
      router_delete_namespaces = True

***

*Configure the DHCP Agent here /etc/neutron/dhcp_agent.ini*

**Legacy OpenvSwitch**

      [DEFAULT]
      verbose = True
      interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
      dhcp_driver = neutron.agent.linux.dhcp.Dnsmasq
      use_namespaces = True
      dhcp_delete_namespaces = True

**Legacy Linux br**

      [DEFAULT]
      verbose = True
      interface_driver = neutron.agent.linux.interface.BridgeInterfaceDriver
      dhcp_driver = neutron.agent.linux.dhcp.Dnsmasq
      use_namespaces = True
      dhcp_delete_namespaces = True

***

*Configure the metadata agent here /etc/neutron/metadata_agent.ini*

      [DEFAULT]
      verbose = True
      nova_metadata_ip = controller
      metadata_proxy_shared_secret = METADATA_SECRET

*Optionally add DNSmasq configuration file into /etc/neutron/dhcp_agent.ini*

      [DEFAULT]
      dnsmasq_config_file = /etc/neutron/dnsmasq-neutron.conf

      *Edit dnsmasq-neutron.conf*

      dhcp-option-force=26,1450 [...]

***

*Start services*

      Open vSwitch
      Open vSwitch agent
      L3 agent
      DHCP agent
      Metadata agent

## Neutron on **compute** node

*As usual*

      net.ipv4.conf.default.rp_filter=0
      net.ipv4.conf.all.rp_filter=0
      net.bridge.bridge-nf-call-iptables=1
      net.bridge.bridge-nf-call-ip6tables=1

*Edit the common config /etc/neutron/neutron.conf*

      [DEFAULT]
      verbose = True

***

*Edit the OpenvSwitch agent (ML2) configuration here /etc/neutron/plugins/ml2/ml2_conf.ini*

      [ovs]
      local_ip = TUNNEL_INTERFACE_IP_ADDRESS
      enable_tunneling = True
      bridge_mappings = vlan:br-vlan

      [agent]
      l2_population = True
      tunnel_types = gre,vxlan

      [securitygroup]
      firewall_driver = neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver
      enable_security_group = True
      enable_ipset = True

***

*Start services*

      Open vSwitch
      Open vSwitch agent

## Verify the setup

- neutron agent-list
- On the network node, verify creation of the qrouter and qdhcp namespaces with *ip netns*

# Neutron setup with HA

# Neutron performance enhancements

# Resources
- http://fr.slideshare.net/danwent/openstack-quantum-intro-os-meetup-32612
- http://docs.openstack.org/networking-guide/deploy.html
- http://docs.openstack.org/networking-guide/config_ml2_plug_in.html
- http://docs.openstack.org/admin-guide-cloud/content/tenant-provider-networks.html
- http://docs.openstack.org/networking-guide/migration_nova_network_to_neutron.html
- https://developer.rackspace.com/blog/neutron-networking-l3-agent/
- https://duckduckgo.com/?q=site%3Adeveloper.rackspace.com%2Fblog+neutron&t=ffsb
