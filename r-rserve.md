## Table of contents

[TOC]

****************************************

### R compilation
    apt-get install -y make gcc g++ gfortran libreadline-dev libcairo2-dev libpango1.0-dev libjpeg8-dev texlive-latex-base texlive-latex-extra
    wget http://mirror.ibcp.fr/pub/CRAN/src/base/R-3/R-3.1.3.tar.gz
    tar xzf R-3.1.3.tar.gz
    ./configure --prefix=/opt/R --enable-R-shlib --with-x=no [--with-blas="-lopenblas"][1] 
    make -j 4 &>compilation.log
    make install
    make check
    /opt/R/bin/R CMD --version
    
[1]:http://www.openblas.net/
    
### R functions
    help("INSTALL")
    help("install.packages")
    installed.packages()

### R addons

### Rserve
    wget http://www.rforge.net/src/contrib/Rserve_1.8-1.tar.gz
    /opt/R/bin/R CMD INSTALL ~/R/Rserve_1.8-1.tar.gz
    /opt/R/bin/R CMD Rserve --version