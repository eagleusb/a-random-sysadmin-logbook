# Table of contents



****************************************

# Oracle JDK
## RaspberryPi B+ deployment
    Download Oracle JDK for ARM here http://www.oracle.com/technetwork/java/javase/downloads/jdk8-arm-downloads-2187472.html
    tar xzvf jdk-8u51-linux-arm-vfp-hflt.tar.gz -C /mnt/usbdrive02/opt/jvm/
    update-alternatives --install /usr/bin/java{c} java{c} /mnt/usbdrive02/opt/jvm/jdk1.8.0_51/bin/java{c} 1

#### Configuration print
    java -XX:+PrintFlagsFinal -version

#### Launch example
    java -d64 -server -XX:+AggressiveOpts -XX:+UseLargePages -Xmn10g  -Xms26g -Xmx26g
    java -d64 -server -XX:+AggressiveOpts -XX:+AggressiveHeap -XX:+UseAES -XX:+UseAESIntrinsics -Xms6G -Xmx6G -XX:MaxHeapSize=6G -XX:-UseGCOverheadLimit -XX:ErrorFile=/var/log/java_error.log -XX:-UsePerfData
    -XX:+AggressiveOpts -Xloggc:/var/log/tomcat7/java_gc.log -XX:+PrintGC -XX:+UseParallelGC -XX:+UseAES -XX:+UseAESIntrinsics -XX:-UseGCOverheadLimit -XX:-UsePerfData -XX:MetaspaceSize=256M -XX:MaxMetaspaceSize=512M -XX:MinHeapFreeRatio=20 -XX:MaxGCPauseMillis=1000

#### Interesting options [+](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html#CBBFHAJA)
    -Xmnsize
    Sets the initial and maximum size (in bytes) of the heap for the young generation (nursery). Append the letter k or K to indicate kilobytes, m     or M to indicate megabytes, g or G to indicate gigabytes.
    Instead of the -Xmn option to set both the initial and maximum size of the heap for the young generation, you can use -XX:NewSize to set the       initial size and -XX:MaxNewSize to set the maximum size.

    -Xmssize
    Sets the initial size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 1 MB. Append the letter k or K to indicate     kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes.

    -Xmxsize
    Specifies the maximum size (in bytes) of the memory allocation pool in bytes. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, -Xms and -Xmx are often set to the same value. See the section "Ergonomics" in Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide at http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html.
    The -Xmx option is equivalent to -XX:MaxHeapSize.

    -XX:+UseHugeTLBFS
    -XX:+UseLargePages
    -XX:+UseSHM

    -XX:+AggressiveHeap

    -XX:InitialHeapSize=size

    -XX:MaxHeapSize=size
    Sets the maximum size (in byes) of the memory allocation pool. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, g or G to indicate gigabytes. The default value is chosen at runtime based on system configuration. For server deployments, -XX:InitialHeapSize and -XX:MaxHeapSize are often set to the same value. See the section "Ergonomics" in Java SE HotSpot Virtual Machine Garbage Collection Tuning Guide at http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/index.html.

    -XX:MaxNewSize=size

    -XX:+UseParallelGC
    -XX:+UseParNewGC
    -XX:+UseConcMarkSweepGC
    -XX:+UseG1GC

    -Xloggc:/var/log/java_gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps
    GC profiling through external tools.

#### Links

    [Oracle Java 8](http://docs.oracle.com/javase/8/)
    [Oracle Java 8 deployment](http://docs.oracle.com/javase/8/docs/technotes/guides/deploy/)
    [Oracle Java GC Tuning Guide](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/)
    [Oracle Java 8 command-line reference](http://docs.oracle.com/javase/8/docs/technotes/tools/unix/intro.html#sthref17)
    [GC profiling GCViewer](https://github.com/chewiebug/GCViewer)
