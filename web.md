## Table of contents

[TOC]

****************************************

### Node.js deployment
    # Note the new setup script name for Node.js v0.12
    curl -sL https://deb.nodesource.com/setup_0.12 | sudo bash -
    sudo apt-get install -y nodejs
    node -v
    npm -v

[See More](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions)

### etherpad deployment
### ARMv7/x64/x86
    apt-get install gzip git curl python libssl-dev pkg-config build-essential
    git clone git://github.com/ether/etherpad-lite.git
    useradd -c "etherpad env user" -m -s /bin/false etherpad
    chown etherpad:etherpad etherpad-lite -R
    sudo -u etherpad etherpad-lite/bin/run.sh 
### ARMv6/RaspberryPi  
    wget https://iojs.org/dist/v1.6.4/iojs-v1.6.4-linux-armv6l.tar.gz
    sudo update-alternatives --install /usr/bin/node node /mnt/usbdrive01/opt/iojs-v1.6.4-linux-armv6l/bin/node 1
    sudo update-alternatives --install /usr/bin/npm npm /mnt/usbdrive01/opt/iojs-v1.6.4-linux-armv6l/bin/npm 1
    sudo update-alternatives --install /usr/bin/iojs iojs /mnt/usbdrive01/opt/iojs-v1.6.4-linux-armv6l/bin/iojs 1
    useradd -c "etherpad env user" -m -s /bin/false etherpad
    sudo -u etherpad etherpad-lite/bin/run.sh
    
    
[See More](https://github.com/ether/etherpad-lite#gnulinux-and-other-unix-like-systems)
    