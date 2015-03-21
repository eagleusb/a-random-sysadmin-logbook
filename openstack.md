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
    glance image-lis
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

### Under the hood of compute
    virsh list --all
    virsh version

### Under the hood of Glance node

    ls -lcth /images/