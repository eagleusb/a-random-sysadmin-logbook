## Table of contents

[TOC]

****************************************

### Oracle JDK
#### Configuration print
    java -XX:+PrintFlagsFinal -version

#### Launch example
    java -d64 -server -XX:+AggressiveOpts -XX:+UseLargePages -Xmn10g  -Xms26g -Xmx26g
    java -server -d64 -XX:+AggressiveOpts -XX:+AggressiveHeap -XX:+UseAES -XX:+UseAESIntrinsics -Xms6G -Xmx6G -XX:MaxHeapSize=6G -XX:-UseGCOverheadLimit -XX:ErrorFile=/var/log/java_error.log -XX:-UsePerfData
    
#### Interesting options
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