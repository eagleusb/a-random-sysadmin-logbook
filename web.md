

****************************************

# Node.js deployment
    # Note the new setup script name for Node.js v0.12
    curl -sL https://deb.nodesource.com/setup_0.12 | sudo bash -
    sudo apt-get install -y nodejs
    node -v
    npm -v

[See More](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions)

# etherpad deployment
## ARMv7/x64/x86
    apt-get install gzip git curl python libssl-dev pkg-config build-essential
    git clone git://github.com/ether/etherpad-lite.git
    useradd -c "etherpad env user" -m -s /bin/false etherpad
    chown etherpad:etherpad etherpad-lite -R
    sudo -u etherpad etherpad-lite/bin/run.sh
## ARMv6/RaspberryPi  
    wget http://node-arm.herokuapp.com/node_latest_armhf.deb
    dpkg -i node_latest_armhf.deb
    git clone git://github.com/ether/etherpad-lite.git
    useradd -c "etherpad env user" -m -s /bin/false etherpad
    chown etherpad:etherpad etherpad-lite -R
    git checkout 1.5.6
    sudo npm install --production --unsafe-perm sqlite3
    screen -dS etherpad "sudo -u etherpad etherpad-lite/bin/run.sh"


[See More](https://github.com/ether/etherpad-lite#gnulinux-and-other-unix-like-systems)

# Ghost deployment
## ARMv6/RaspberryPi
    screen -AmdS ghost
    curl -L https://ghost.org/zip/ghost-latest.zip -o ghost.zip
    unzip -u ghost.zip
    npm install --production --verbose
    useradd -c "ghost user env" -M -s /bin/false ghost && chown -R ghost:ghost $PWD
    sudo -u ghost npm start --production --verbose
