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

# Revolution Analytics RRO 8.0.2
    apt-get build-dep r-base
    tar xzf RRO-8.0.2-Beta-Ubuntu-14.04.x86_64.tar.gz
    ./install.sh
    ./RevoMath.sh --nolicense
    R CMD INSTALL --library="/usr/lib64/RRO-8.0.2/R-3.1.2/lib/R/library" RevoUtilsMath.tar.gz
    R CMD INSTALL --library="/usr/lib64/RRO-8.0.2/R-3.1.2/lib/R/library" Rserve_1.8-2.tar.gz
    export ALLOW_WGCNA_THREADS=2
    R CMD Rserve --vanilla --RS-conf /etc/Rserve.conf

# Rserve over R
    wget http://www.rforge.net/src/contrib/Rserve_1.8-1.tar.gz
    R CMD INSTALL Rserve_1.8-1.tar.gz
    R CMD Rserve --version

## Start Rserve in debug
    R CMD Rserve.dbg --vanilla --RS-conf /etc/Rserve.conf

# R functions examples
    help("INSTALL")
    help("install.packages")
    installed.packages()
    install.packages('Rserve',,'http://www.rforge.net/')
    update.packages(checkBuilt=TRUE)
    .libPaths()
    .Library
    getRversion()
    source("http://bioconductor.org/biocLite.R")
    chooseCRANmirror()
    library(Cairo)
    gc()

# R script execution
    R -f R_script.R

# R links

- Rserve configuration reference: https://github.com/s-u/Rserve/wiki/rserve.conf
- R command line arguments: http://cran.r-project.org/doc/manuals/r-release/R-intro.html#Invoking-R
- R memory management : https://stat.ethz.ch/pipermail/r-devel/2015-January/070439.html
- Rserve 1.8 fork feature : http://comments.gmane.org/gmane.comp.lang.r.rosuda.devel/1816
- Rserve changelog (lots of information regardint to conf file): http://cran.r-project.org/web/packages/Rserve/NEWS
- Main R website: http://www.r-project.org/
- Main Rserve website: http://www.rforge.net/Rserve/
- Revolution Analytics RRO: http://mran.revolutionanalytics.com/
- Revolution Analytics deployR: http://deployr.revolutionanalytics.com/
