# Build Guide

1. Enter `mmtk-lxr/openjdk/`.
2. `sh configure --disable-warnings-as-errors --with-debug-level=release --with-target-bits=64 --disable-zip-debug-info --with-jvm-features=shenandoahgc`
3. `make --no-print-directory CONF=linux-x86_64-normal-server-release THIRD_PARTY_HEAP=$PWD/../mmtk-openjdk/openjdk` 加 GC_FEATURES=log_work_packets 类似的环境变量可以覆写Cargo.toml中的[features]项。
4. `MMTK_PLAN=Immix TRACE_THRESHOLD2=10 LOCK_FREE_BLOCKS=32 MAX_SURVIVAL_MB=256 SURVIVAL_PREDICTOR_WEIGHTED=1 ./mmtk-openjdk/repos/openjdk/build/linux-x86_64-normal-server-release/jdk/bin/java -XX:MetaspaceSize=1G -XX:-UseBiasedLocking -XX:-TieredCompilation -XX:+UnlockDiagnosticVMOptions -XX:-InlineObjectCopy -Xms100M -Xmx100M -XX:+UseThirdPartyHeap Hello`


MMTK_PLAN=Immix TRACE_THRESHOLD2=10 LOCK_FREE_BLOCKS=32 MAX_SURVIVAL_MB=256 SURVIVAL_PREDICTOR_WEIGHTED=1 ./openjdk/build/linux-x86_64-normal-server-release/jdk/bin/java -XX:MetaspaceSize=1G -XX:-UseBiasedLocking -XX:-TieredCompilation -XX:+UnlockDiagnosticVMOptions -XX:-InlineObjectCopy -Xms100M -Xmx100M -XX:+UseThirdPartyHeap Hello


sh configure --disable-warnings-as-errors --with-debug-level=fastdebug --with-target-bits=64 --with-jvm-features=lxr
make CONF=linux-x86_64-normal-server-fastdebug THIRD_PARTY_HEAP=$PWD/../mmtk-openjdk/openjdk images
./build/linux-x86_64-normal-server-fastdebug/jdk/bin/java -XX:+UseThirdPartyHeap HelloWorld

sh configure --disable-warnings-as-errors --with-debug-level=release --with-target-bits=64 --with-jvm-features=lxr
make CONF=linux-x86_64-normal-server-release THIRD_PARTY_HEAP=$PWD/../mmtk-openjdk/openjdk images
./build/linux-x86_64-normal-server-release/jdk/bin/java -XX:+UseThirdPartyHeap HelloWorld




MMTK_PLAN=LXR MMTK_VERBOSE=3 TRACE_THRESHOLD2=10 LOCK_FREE_BLOCKS=32 MAX_SURVIVAL_MB=256 SURVIVAL_PREDICTOR_WEIGHTED=1 ./openjdk/build/linux-x86_64-normal-server-release/jdk/bin/java -Xms32g -XX:-UseDynamicNumberOfGCThreads -XX:MetaspaceSize=256m -XX:-UseNUMA -XX:-UseCompressedOops -XX:+ExitOnOutOfMemoryError -Xlog:gc*=info,gc+stats=off:gc.log:time,uptime,tid,level,tags:filecount=0 -XX:-UseBiasedLocking -XX:-TieredCompilation -XX:+UnlockDiagnosticVMOptions -XX:-InlineObjectCopy -XX:+UseThirdPartyHeap -XX:ThirdPartyHeapOptions=plan=LXR Hello