## Table of contents

[TOC]

****************************************

### tcpdump

### iptables SNAT/DNAT

### ip forwarding
    echo 1 > /proc/sys/net/ipv4/ip_forward
    sysctl –w net.ipv4.ip_forward=1
    sysctl –w net.ipv6.conf.all.forwarding=1
    
#### ARP Proxy
    echo 1 > /proc/sys/net/ipv4/conf/all/proxy_arp
    echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce
    echo 1 > /proc/sys/net/ipv6/conf/all/proxy_ndp

### RP filter
    sysctl -w "net.ipv4.conf.all.rp_filter=1"
    [Cisco Press Reverse Path Filtering][1]
    
[1]:http://www.ciscopress.com/articles/article.asp?p=1725270
### ICMP redirects
    sysctl -w net.ipv4.conf.all.accept_redirects=0
    sysctl -w net.ipv6.conf.all.accept_redirects=0
    [Cisco Press][2]
    
[2]:http://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13714-43.html
### sysctl

### ethtool

### bonding
    modprobe bonding
    echo bonding >> /etc/modules

    #bonding des interfaces phy
    auto bond0
    iface bond0 inet manual
            slaves eth0 eth1
            bond_mode active-backup
            bond_miimon 100
            
    cat /proc/net/bonding/bond0
### bridge
    auto vmbr100
    iface vmbr100 inet static
            address 192.168.200.101
            netmask 255.255.255.0
            gateway 192.168.200.1
            #pointopoint 192.168.200.1
            bridge_ports bond0
            bridge_stp off
            bridge_fd 0
### vlan & routing multiple interfaces
    echo 8021q >> /etc/modules
    modprobe 8021q
    auto eth0.100
    iface eth0.100  inet static
        address 147.210.6.2
        netmask 255.255.255.0
        network 147.210.6.0
        broadcast 147.210.6.255
        gateway 147.210.6.254
    echo "201 VLAN100" >> /etc/iproute2/rt_tables
    ip rule add from 147.210.6.0/24 table VLAN100
    ip route add 147.210.6.0/24 dev eth0.100 table VLAN100
    ip route add default via 147.210.6.254 dev eth0.100 table VLAN100
    ip route flush cache
    
[See more][3]

[3]:http://lartc.org/howto/
### routing
    route add 192.168.200.151 dev vmbr0
    ip address show
    ip route show
    ip rule ls
    ip link
