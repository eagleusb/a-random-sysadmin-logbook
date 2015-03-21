### Kernel compilation

> necessary packages : build-essential libncurses5-dev

    cp /boot/config-bla /usr/src/linux-source*/.config
    make menuconfig
    `old way` : fakeroot make-kpkg --append-to-version "-core2" --revision "1" --us --uc --initrd kernel_image kernel_headers
    `new way`[1] : scripts/config --disable DEBUG_INFO; make clean; make -j6 deb-pkg LOCALVERSION=-xeon3060 KDEB_PKGVERSION=1

[1]: http://debian-handbook.info/browse/fr-FR/stable/sect.kernel-compilation.html


### SSHd Auth

    chmod 700 .ssh
    chmod 600 authorized_keys

### MAJ les libs

    ldconfig

### 3proxy

    apt-get install libssl-dev
    make -f Makefile.Linux

### Ethernet interface

    # The primary network interface
     allow-hotplug eth0
     auto eth0
     iface eth0 inet static
        address 79.129.248.82
        netmask 255.255.255.240
        network 79.129.248.80
        broadcast 79.129.248.95
        gateway 79.129.248.81
        # dns-* options are implemented by the resolvconf package, if installed
        dns-nameservers 88.117.198.200 88.117.237.100 88.117.196.200

Virtual interface

    iface eth1:0 inet static
    address 200.21.23.155
    netmask 255.255.255.0
    gateway 200.21.23.1
    dns-nameservers 4.2.2.1 8.8.8.8 8.8.4.4

### RAID logiciel
 

    mdadm --create --verbose /dev/md0 --level=0 --chunk=128 --raid-devices=3 /dev/sd[bcd]1
    mdadm --detail --scan --verbose >> /etc/mdadm/mdadm.conf

#### Directories
- /etc/default/mdadm
- /etc/mdadm/mdadm.conf
- /usr/share/mdadm/*

### Cryptsetup
#### Plain mode

    cryptsetup create plainvolume -c aes-ecb-plain -s 128 /dev/md0

#### LUKS

    cryptsetup luksFormat -c aes-xts-plain64 -h sha1 -s 256 -y /dev/sdb1
    cryptsetup luksOpen /dev/sdb1 /home/topkekvolume
    
