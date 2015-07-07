# Neutron

# ML2 plug-in

![Neutron ML2 architecture](http://aqorn.com/wp-content/uploads/2014/11/ml2-arch1.png)

## Type drivers
TypeDrivers maintain any needed type-specific network state, and perform provider network validation and tenant network allocation
- Flat
- Local
- VLAN
- GRE
- VxLan
## Mechanisms
The MechanismDriver is responsible for taking the information established by the TypeDriver and ensuring that it is properly applied given the specific networking mechanisms that have been enabled
- Linux bridges
- OpenVSwitch
- Cisco Nexus...

# Neutron networking node
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)
- L3 agent i.e. qrouter (iptables) with different namespaces per tenant/project
- DHCP agent
- Metadata agent

![Networking node overview with legacy Linux br](http://docs.openstack.org/networking-guide/_images/scenario-legacy-lb-network1.png)
![Networking node overview with new OpenVSwitch](http://docs.openstack.org/networking-guide/_images/scenario-legacy-ovs-network1.png)

# Neutron controller node
- Neutron server service
- ML2 plug-in

# Neutron compute node
- ML2 plug-in
  - OpenVSwitch service,agent or Linux bridges (legacy)


#Resources
http://fr.slideshare.net/danwent/openstack-quantum-intro-os-meetup-32612
http://docs.openstack.org/networking-guide/deploy.html
http://docs.openstack.org/networking-guide/config_ml2_plug_in.html
http://docs.openstack.org/admin-guide-cloud/content/tenant-provider-networks.html
http://docs.openstack.org/networking-guide/migration_nova_network_to_neutron.html
