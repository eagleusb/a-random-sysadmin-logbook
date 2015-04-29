## Table of contents

[TOC]

****************************************

### PackStack packages
    openstack-keystone-2014.2.1-1.el7.centos.noarch
    openstack-nova-common-2014.2.1-1.el7.centos.noarch
    openstack-nova-console-2014.2.1-1.el7.centos.noarch
    openstack-nova-scheduler-2014.2.1-1.el7.centos.noarch
    openstack-nova-conductor-2014.2.1-1.el7.centos.noarch
    openstack-nova-cert-2014.2.1-1.el7.centos.noarch
    openstack-cinder-2014.2.1-2.el7.centos.noarch
    openstack-nova-compute-2014.2.1-1.el7.centos.noarch
    openstack-glance-2014.2.1-2.el7.centos.noarch
    openstack-nova-api-2014.2.1-1.el7.centos.noarch

### Glance API
    glance image-list
    glance image-delete bubuntu
    glance image-create --copy-from http://uec-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img --is-public True --disk-format qcow2 --container-format bare --name bubuntu
    glance -k image-create --name IMPORT-BACKUP-TOPKEKVM --disk-format qcow2 --container-format bare --file b6d9e297-e77b-4c78-a667-b780b926b97b

### Nova API
    nova list --all-tenants
    nova boot --flavor m1.large --image official-ubuntu-14.04-x86_64 --key-name etriks-vms --security-groups web,ssh-ext,database,munin TOPKEKVM
    nova boot --flavor m1.medium --image SNAPSHOT --key-name DATKEY --nic net-id=8b345b99-c8f3-42a4-a2b2-277d910dd798,v4-fixed-ip=150.50.17.50 TOPKEKVM
    nova quota-update 2d5a7a7585be44b7bf4db4b79402715d --cores 100
    nova quota-show
    nova network-list
    nova network-show ultrafast-net
    nova reboot --hard <uuid> # recreates libvirt XML and hard reboot

### Keystone
    keystone tenant-list
    keystone service-list
#### Create typical service
    keystone user-create --name cinder --pass PASS
    keystone user-role-add --user cinder --role admin --tenant services
    keystone service-create --name cinder --type volume --description "Cinder Volume Services"
    keystone endpoint-create --service cinder --publicurl "http://IP:8776/v1/\$(tenant_id)s" --adminurl "http://IP:8776/v1/\$(tenant_id)s" --internalurl "http://IP:8776/v1/\$(tenant_id)s"

### Under the hood of Nova Compute node
    virsh list --all
    virsh version

### Under the hood of Cinder node


### Under the hood of Glance node
    ls -lcth /images/
    qemu-img info /images/uuid

### Under the hood of metadata & cloud-init
    ip a|grep -i 169.254.169.254
    iptables -t nat -L -n|grep -i 169.254.169.254
    curl http://169.254.169.254
    tcpdump -n -i any host dst 169.254.169.254 and tcp port 80
    iptables -t nat -A PREROUTING -d 169.254.169.254 -p tcp -m tcp --dport 80 -j DNAT --to-destination NOVA-NET-IP:8775[1]

[1]:https://github.com/openstack/nova/blob/52877cddaa1612aad24f525c55cfc03c10450360/nova/network/linux_net.py

### Schemas
#### Network diagrams
![Tenants network overview](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/4971.png)
![Neutron deployment architecture](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/2476.png)
![Neutron topology example](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/5113.png)
![VXVLAN/GRE L2 tunneling](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/4780.png)
![VMWare NSX topology overview](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/5194.png)
#### Block Storage Architecture
![Block storage aka Cinder](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/2638.png)
