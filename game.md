## Table of contents

[TOC]

****************************************

### CS:GO deployment
    apt-get install lib32gcc1
    useradd -m steam
    su - steam
    wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
    tar xzf steamcmd_linux.tar.gz
    ./steamcmd.sh +login anonymous +force_install_dir ~/csgo_srv +app_update 740 +quit