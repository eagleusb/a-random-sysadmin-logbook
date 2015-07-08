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
- VxLan

## ML2 Mechanisms
The MechanismDriver is responsible for taking the information established by the TypeDriver and ensuring that it is properly applied given the specific networking mechanisms that have been enabled
- Linux bridges
- OpenVSwitch
- Cisco Nexus...

# L3 agent
    Neutron has an API extension to allow administrators and tenants to create "routers" that connect to L2 networks. Known as the "neutron-l3-agent", it uses the Linux IP stack and iptables to perform L3 forwarding and NAT. In order to support multiple routers with potentially overlapping IP addresses, neutron-l3-agent defaults to using Linux network namespaces to provide isolated forwarding contexts.

# Neutron networking node services
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)
- L3 agent i.e. qrouter (iptables) with different namespaces per tenant/project
- DHCP agent
- Metadata agent

****

*Networking node overview with legacy Linux br*

![Networking node overview with legacy Linux br](http://docs.openstack.org/networking-guide/_images/scenario-legacy-lb-network1.png)

****

*Networking node overview with new OpenVSwitch*

![Networking node overview with new OpenVSwitch](http://docs.openstack.org/networking-guide/_images/scenario-legacy-ovs-network1.png)

****

# Neutron controller node services
- Neutron server service
- ML2 plug-in

# Neutron compute node services
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)


# Resources
- http://fr.slideshare.net/danwent/openstack-quantum-intro-os-meetup-32612
- http://docs.openstack.org/networking-guide/deploy.html
- http://docs.openstack.org/networking-guide/config_ml2_plug_in.html
- http://docs.openstack.org/admin-guide-cloud/content/tenant-provider-networks.html
- http://docs.openstack.org/networking-guide/migration_nova_network_to_neutron.html
- https://developer.rackspace.com/blog/neutron-networking-l3-agent/
- https://duckduckgo.com/?q=site%3Adeveloper.rackspace.com%2Fblog+neutron&t=ffsb
