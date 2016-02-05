<!-- TOC depth:6 withLinks:1 updateOnSave:1 -->

- [Glance API](#glance-api)
- [Nova API](#nova-api)
- [Keystone](#keystone)
	- [Create typical service](#create-typical-service)
- [Under the hood of Nova Compute node](#under-the-hood-of-nova-compute-node)
- [Under the hood of Cinder node](#under-the-hood-of-cinder-node)
- [Under the hood of Glance node](#under-the-hood-of-glance-node)
- [Under the hood of metadata & cloud-init](#under-the-hood-of-metadata-cloud-init)
- [Schemas](#schemas)
	- [Network diagrams](#network-diagrams)
	- [Block Storage Architecture](#block-storage-architecture)
- [Links](#links)
	- [Libvirt](#libvirt)

<!-- /TOC -->
****************************************

# Glance API
	glance image-list
	glance image-delete bubuntu
	glance image-create --copy-from http://uec-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img --is-public True --disk-format qcow2 --container-format bare --name bubuntu
	glance -k image-create --name IMPORT-BACKUP-TOPKEKVM --disk-format qcow2 --container-format bare --file b6d9e297-e77b-4c78-a667-b780b926b97b

# Nova API
	nova list --all-tenants
	nova boot --flavor m1.large --image official-ubuntu-14.04-x86_64 --key-name etriks-vms --security-groups web,ssh-ext,database,munin TOPKEKVM
	nova boot --flavor m1.medium --image SNAPSHOT --key-name DATKEY --nic net-id=8b345b99-c8f3-42a4-a2b2-277d910dd798,v4-fixed-ip=150.50.17.50 TOPKEKVM
	nova quota-update 2d5a7a7585be44b7bf4db4b79402715d --cores 100
	nova quota-show
	nova network-list
	nova network-show ultrafast-net
	nova reboot --hard <uuid> # recreates libvirt XML and hard reboot

# Keystone
	keystone tenant-list
	keystone service-list
## Create typical service
	keystone user-create --name cinder --pass PASS
	keystone user-role-add --user cinder --role admin --tenant services
	keystone service-create --name cinder --type volume --description "Cinder Volume Services"
	keystone endpoint-create --service cinder --publicurl "http://IP:8776/v1/\$(tenant_id)s" --adminurl "http://IP:8776/v1/\$(tenant_id)s" --internalurl "http://IP:8776/v1/\$(tenant_id)s"

# Under the hood of Nova Compute node
	virsh list --all
	virsh version
	for i in *-*-*-*-*/disk;do qemu-img info $i | grep "backing file" >> used-images.log;done;
	for i in /instances/_base/*;do if [[ $(grep -c "$i" /instances/used-images.log) == 0 ]];then echo "$i is unused" ;fi;done;
## Nova migrate
	qemu-img convertion
	rsync --sparse --compress /instances/01b2f270-f62c-47ea-ba0f-a38f83a0fa57_resize/disk_rbase IP_DST:/instances/01b2f270-f62c-47ea-ba0f-a38f83a0fa57/disk
	\_ ssh IP_DST rsync --server -Sze.Lsf . /instances/01b2f270-f62c-47ea-ba0f-a38f83a0fa57/disk
## libvirt
	tail -f /var/log/libvirt/qemu/instance-\*.log

# Under the hood of Cinder node
## SQL DB ops
	SHOW databases;
	USE cinder;
	DESC volumes;
	SELECT * FROM volumes where id='93d405ac-83cc-45e5-bccf-ba792c36156d';
	SELECT id,instance_uuid,host,status,attach_status,attached_host,migration_status FROM volumes where display_name like 'lad-test%';
	SELECT id,display_name FROM volumes WHERE provider_location LIKE '1.1.1.3%' AND deleted=0 limit 50;
### Force delete a volume
	UPDATE volumes SET deleted=1,migration_status=NULL,provider_location=NULL,provider_auth=NULL,mountpoint=NULL,instance_uuid=NULL,deleted_at='2015-10-02 10:39:00',attach_status='detached',status='deleted' WHERE id='ecd9ccf8-3c70-48dd-82c5-de076c57cfa0';
### Reset status after a failed migration
	UPDATE volumes SET migration_status=NULL,provider_location=NULL,provider_auth=NULL,migration_status=NULL,mountpoint=NULL,instance_uuid=NULL,attach_status='detached' WHERE deleted=1 and provider_location LIKE '1.1.1.3%' and migration_status LIKE 'target%';
	UPDATE instances SET host='compute-hostname.domain',node='compute-hostname.domain' WHERE uuid='vm_uuid' and project_id='project_uuid';
### Force delete attached volume
	DELETE FROM volume_attachment where instance_uuid = 'f3139585-a07b-453a-b930-6459899891df';
	UPDATE volumes SET status='available',attach_status='detached' where id='f7d529f8-ca91-4a1e-9c07-8f4ce9a20bf6';
	USE nova;
	DELETE FROM block_device_mapping WHERE instance_uuid='f3139585-a07b-453a-b930-6459899891df' AND volume_id='f7d529f8-ca91-4a1e-9c07-8f4ce9a20bf6';
## iSCSI
### Delete a stucked iscsi device
	lsscsi | grep LIO
	cat /proc/scsi/scsi
	cat /sys/block/sdb/device/state
	echo 1 > /sys/block/sdb/device/delete
### iscsiadm
	iscsiadm -m session -P1 to 3
	iscsiadm -m discoverydb -t sendtargets -p cindernodeIP --discover
	iscsiadm -m discoverydb -t sendtargets -p cindernodeIP -o new -o delete --discover
	iscsiadm -m node -T iqn.2010-10.org.openstack:volume-5d51f748-aec8-4da7-a3d2-1083afb720b6 -o delete
	iscsiadm -m node -U all
	iscsiadm -m session [ ]-r sessionid ] [ -u ]
	[iscsiadm man]:http://www.open-iscsi.org/docs/README
### LIO
	targetcli ls
	cd tpg1/acls/iqn.1994-05.com.redhat:1f3ec6a073a8/
	get auth
### TGT
	tgt-admin -s or tgtadm --lld iscsi --op show --mode target

# Under the hood of Glance node
		ls -lcth /images/
		qemu-img info /images/uuid

# Under the hood of metadata & cloud-init
		ip a|grep -i 169.254.169.254
		iptables -t nat -L -n|grep -i 169.254.169.254
		curl http://169.254.169.254
		tcpdump -n -i any host dst 169.254.169.254 and tcp port 80
		iptables -t nat -A PREROUTING -d 169.254.169.254 -p tcp -m tcp --dport 80 -j DNAT --to-destination NOVA-NET-IP:8775[1]

[1]:https://github.com/openstack/nova/blob/52877cddaa1612aad24f525c55cfc03c10450360/nova/network/linux_net.py

# Schemas
## Network diagrams
Tenants network overview

![Tenants network overview](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/4971.png)

Neutron deployment architecture

![Neutron deployment architecture](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/2476.png)

Neutron topology example

![Neutron topology example](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/5113.png)

VXVLAN/GRE L2 tunneling

![VXVLAN/GRE L2 tunneling](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/4780.png)

VMWare NSX topology overview

![VMWare NSX topology overview](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/5194.png)
## Block Storage Architecture

Block storage aka Cinder

![Block storage aka Cinder](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/6/html-single/Deploying_OpenStack_Learning_Environments/images/2638.png)

***************

# Links
## OpenStack network
[Latest Neutron networking concepts](http://docs.openstack.org/networking-guide/)
[Network topology design](http://docs.openstack.org/openstack-ops/content/network_design.html#network_topology)
[Neutron wiki](https://wiki.openstack.org/wiki/Neutron)
## Libvirt
[The life of an OpenStack libvirt image](http://www.pixelbeat.org/docs/openstack_libvirt_images/)
