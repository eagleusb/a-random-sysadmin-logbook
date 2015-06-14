<!-- TOC depth:6 withLinks:1 updateOnSave:1 -->
- [Classic R compilation](#classic-r-compilation)
- [Revolution Analytics R fork](#revolution-analytics-r-fork)
- [R functions](#r-functions)
- [R addons](#r-addons)
- [Rserve](#rserve)
<!-- /TOC -->
****************************************

# Classic R compilation
    apt-get install -y make gcc g++ gfortran libreadline-dev libcairo2-dev libpango1.0-dev libjpeg8-dev texlive-latex-base texlive-latex-extra
    wget http://mirror.ibcp.fr/pub/CRAN/src/base/R-3/R-3.1.3.tar.gz
    tar xzf R-3.1.3.tar.gz
    ./configure --prefix=/opt/R --enable-R-shlib --with-x=no [--with-blas="-lopenblas"][1]
    make -j 4 &>compilation.log
    make install
    make check
    /opt/R/bin/R CMD --version

[1]:http://www.openblas.net/

# Revolution Analytics R fork

# R functions
    help("INSTALL")
    help("install.packages")
    installed.packages()
    install.packages('Rserve',,'http://www.rforge.net/')
    update.packages(checkBuilt=TRUE)
    chooseCRANmirror()
    getRversion()
    library(Cairo)
    gc()

# R addons
    R -f R_script.R

# Rserve
    wget http://www.rforge.net/src/contrib/Rserve_1.8-1.tar.gz
    R CMD INSTALL ~/R/Rserve_1.8-1.tar.gz
    R CMD Rserve --version
