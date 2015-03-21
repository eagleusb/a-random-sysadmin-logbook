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
    
### 2tb+ partition

#### Non optimal
    parted /dev/sdx
    mklabel gpt
    mkpart primary ext4 0 6000GB
    
#### Optimal alignment
    parted -a optimal /dev/sdc mklabel gpt
    parted -a optimal /dev/sdc mkpart primary 0% 100%
    
### HDD benchmark
    dd if=/dev/zero of=/home/gl/site/incoming/speed bs=4096k count=1024k
    dd if=/dev/zero of=test bs=4096k oflag=direct
    hdparm -tT /dev/sda
    
### OpenSSL
    ./config --prefix=/topsecureapp/lel --openssldir=/topsecureapp/lel/openssl
    ./config zlib --prefix=/topsecureapp/lel --openssldir=/topsecureapp/lel/openssl
    make && make test && make install
    
#### RSA
    openssl genrsa -out maclef.key 1024
    openssl req -new -x509 -days 900 -key maclef.key -out moncertif.crt
    cat maclef.key moncertif.crt > finalcrt.pem

#### DSA
    openssl dsaparam -rand temp -out temp.dsaparam 1024
    openssl dhparam -rand temp -out temp.dh 1024
    openssl gendsa -aes128 -out maclef.key temp.dsaparam
    openssl req -new -x509 -days 900 -key maclef.key -out moncertif.crt
    cat maclef.key moncertif.crt temp.dh > finalcrt.pem
    
### SSHfs
    usermod -G fuse -a root
    sshfs supergooduser@dasanbox:/volume1/site /home/bigvolumebro -p 2222 -o allow_other,reconnect
