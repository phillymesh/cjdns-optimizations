# README

This document was created to track the performance of CJDNS builds on various platforms, with and without compilation flags. Each build utilizes gcc, while the CJDNS version is noted in each case.

Note: I am by no means an optimation update. If anything is incorrect or could be done better, feel free to open an issue or pull request.

## Orange Pi Zero

Both builds were performed on Armbian, Armbian_5.25_Orangepizero_Debian_jessie_default_3.4.113.img using cjdns-v19.1

```
$ uname -a
Linux orangepizero 3.4.113-sun8i #10 SMP PREEMPT Thu Feb 23 19:55:00 CET 2017 armv7l GNU/Linux

$ gcc --version | grep "gcc" 
gcc (Debian 4.9.2-10) 4.9.2 

$ ./cjdroute --version 
Cjdns version: cjdns-v19.1
Cjdns protocol version: 19
```

### Default Build

#### Bench

```
$ ./do

$ ./cjdroute --bench | grep "per second" 
1488588835 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 13791ms. 87013 kilobits per second 
1488588839 INFO Benchmark.c:62 Benchmark Switching in 3493ms. 58631 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488588874 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 13799ms. 86962 kilobits per second 
1488588878 INFO Benchmark.c:62 Benchmark Switching in 3496ms. 58581 packets per second 
$ ./cjdroute --bench | grep "per second"
1488588898 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 13820ms. 86830 kilobits per second
1488588902 INFO Benchmark.c:62 Benchmark Switching in 3506ms. 58414 packets per second
```

#### Throughput

```
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN
[ ID] Interval        Transfer   Bandwidth      Retr 
[  4] 0.00-120.00 sec 290 MBytes 20.3 Mbits/sec 165  sender
[  4] 0.00-120.00 sec 290 MBytes 20.3 Mbits/sec      receiver
```

### Optimized Build

#### Bench

```
$ CFLAGS="-s -static -Wall -march=armv7ve -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard -mtune=cortex-a7 -fomit-frame-pointer -marm" ./do

$ ./cjdroute --bench | grep "per second" 
1488589319 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6134ms. 195630 kilobits per second 
1488589322 INFO Benchmark.c:62 Benchmark Switching in 2848ms. 71910 packets per second
$ ./cjdroute --bench | grep "per second" 
1488589341 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6140ms. 195439 kilobits per second 
1488589344 INFO Benchmark.c:62 Benchmark Switching in 2846ms. 71960 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488589365 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6141ms. 195407 kilobits per second
1488589368 INFO Benchmark.c:62 Benchmark Switching in 2847ms. 71935 packets per second
```

#### Throughput

```
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN
[ ID] Interval        Transfer   Bandwidth      Retr 
[  4] 0.00-120.00 sec 366 MBytes 25.6 Mbits/sec 141  sender
[  4] 0.00-120.00 sec 366 MBytes 25.6 Mbits/sec      receiver
```

### Conclusion

The Orange Pi Zero retails for $6.99 USD, so the estimated cost is $0.27 per Mbit.

## Pine A64

Both builds were performed on Armbian, Armbian_5.25_Pine64_Debian_jessie_default_3.10.104.img using cjdns-v19.1

``` 
$ uname -a
Linux pine64 3.10.104-pine64 #15 SMP PREEMPT Fri Feb 3 18:12:41 CET 2017 aarch64 GNU/Linux

$ gcc --version | grep "gcc" 
gcc (Debian/Linaro 4.9.2-10) 4.9.2 

$ ./cjdroute --version Cjdns version: 
cjdns-v19.1
Cjdns protocol version: 19
```

### Default Build

#### Bench

```
$ Seccomp_NO=1 ./do

$ ./cjdroute --bench | grep "per second" 
1488587708 INFO 
Benchmark.c:62 Benchmark salsa20/poly1305 in 6210ms. 193236 kilobits per second 
1488587710 INFO Benchmark.c:62 Benchmark Switching in 1735ms. 118040 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488587744 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6204ms. 193423 kilobits per second 
1488587746 INFO Benchmark.c:62 Benchmark Switching in 1734ms. 118108 packets per second
$ ./cjdroute --bench | grep "per second" 
1488587784 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6211ms. 193205 kilobits per second
1488587785 INFO Benchmark.c:62 Benchmark Switching in 1732ms. 118244 packets per second
```

#### Throughput

```
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN 
[ ID] Interval        Transfer   Bandwidth      Retr 
[  4] 0.00-120.00 sec 346 MBytes 24.2 Mbits/sec 137  sender
[  4] 0.00-120.00 sec 346 MBytes 24.2 Mbits/sec      receiver
```

### Optimized Build

#### Bench

The Pine A64 is advertised to have NEON support and hard-float, but gcc complains if they are specified

```
$ Seccomp_NO=1 CFLAGS="-s -static -Wall -march=armv8-a+crc+crypto -mcpu=cortex-a53 -ftree-vectorize -mtune=cortex-a53 -fomit-frame-pointer" ./do

$ ./cjdroute --bench | grep "per second"
1488587399 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6101ms. 196689 kilobits per second 
1488587401 INFO Benchmark.c:62 Benchmark Switching in 1714ms. 119486 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488587423 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6099ms. 196753 kilobits per second 
1488587425 INFO Benchmark.c:62 Benchmark Switching in 1718ms. 119208 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488587454 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 6100ms. 196721 kilobits per second 
1488587456 INFO Benchmark.c:62 Benchmark Switching in 1713ms. 119556 packets per second                                   
```

#### Throughput

``` 
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN
[ ID] Interval        Transfer   Bandwidth      Retr 
[  4] 0.00-120.00 sec 353 MBytes 24.7 Mbits/sec 127  sender
[  4] 0.00-120.00 sec 353 MBytes 24.7 Mbits/sec      receiver
```

### Conclusion

The Pine A64 retails for $15.00 USD, so the estimated cost is $0.61 per Mbit. 

## BeagleBone Black

Both builds were performed on Debian IoT, the official headless Debian for BeagleBone, bone-debian-8.6-iot-armhf-2016-12-09-4gb.img using cjdns-v19.1

``` 
$ uname -a
Linux beaglebone 4.4.36-ti-r72 #1 SMP Wed Dec 7 22:29:53 UTC 2016 armv7l GNU/Linux

$ gcc --version | grep "gcc" 
gcc (Debian 4.9.2-10) 4.9.2 

$ ./cjdroute --version 
Cjdns version: cjdns-v19.1
Cjdns protocol version: 19
```

### Default Build

#### Bench

```
$ ./do

$ ./cjdroute --bench | grep "per second" 
1488589992 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 34428ms. 34855 kilobits per second 
1488589996 INFO Benchmark.c:62 Benchmark Switching in 4332ms. 47276 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488590046 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 34419ms. 34864 kilobits per second 
1488590050 INFO Benchmark.c:62 Benchmark Switching in 4319ms. 47418 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488590207 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 34388ms. 34895 kilobits per second
1488590211 INFO Benchmark.c:62 Benchmark Switching in 4336ms. 47232 packets per second
```

#### Throughput

```
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN
[ ID] Interval        Transfer    Bandwidth      Retr 
[  4] 0.00-120.00 sec 88.1 MBytes 6.16 Mbits/sec 160  sender
[  4] 0.00-120.00 sec 87.9 MBytes 6.14 Mbits/sec      receiver
```

### Optimized Build

#### Bench

```
$ CFLAGS="-s -static -Wall -march=armv7-a -mcpu=cortex-a8 -mtune=cortex-a8 -marm -mfloat-abi=hard -mfpu=neon -fomit-frame-pointer" ./do

$ ./cjdroute --bench | grep "per second" 
1488590519 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 3526ms. 340328 kilobits per second 
1488590522 INFO Benchmark.c:62 Benchmark Switching in 2955ms. 69306 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488590558 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 3523ms. 340618 kilobits per second 
1488590561 INFO Benchmark.c:62 Benchmark Switching in 2925ms. 70017 packets per second 
$ ./cjdroute --bench | grep "per second"
1488590588 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 3519ms. 341005 kilobits per second
1488590590 INFO Benchmark.c:62 Benchmark Switching in 2918ms. 70185 packets per second
```

#### Throughput

``` 
$ iperf3 -t120 -c CJDNS_PEER_IP_ON_LAN 
[ ID] Interval        Transfer   Bandwidth      Retr 
[  5] 0.00-120.06 sec 172 MBytes 12.0 Mbits/sec 63   sender
[  5] 0.00-120.06 sec 172 MBytes 12.0 Mbits/sec      receiver
```

### Conclusion

The BeagleBone black retails for $45.00 USD, so the estimated cost is $3.75 per Mbit.

## Raspberry Pi 2

Both builds were performed on DietPi, using DietPi_v145_RPi-armv6-(Jessie).img using cjdns-v19.1

```
$ uname -a 
Linux DietPi 4.4.44-v7+ #2 SMP Sat Feb 11 16:07:35 UTC 2017 armv7l GNU/Linux

$ gcc --version | grep "gcc" 
gcc (Raspbian 4.9.2-10) 4.9.2

$ ./cjdroute --version 
Cjdns version: cjdns-v19.1
Cjdns protocol version: 19
```

### Default Build

#### Bench

```
$ Seccomp_NO=1 ./do

$ ./cjdroute --bench | grep "per second" 
1488765477 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 16807ms. 71398 kilobits per second 
1488765482 INFO Benchmark.c:62 Benchmark Switching in 4551ms. 45001 packets per second  
$ ./cjdroute --bench | grep "per second" 
1488765562 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 16802ms. 71420 kilobits per second 
1488765566 INFO Benchmark.c:62 Benchmark Switching in 4530ms. 45209 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488765613 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 16792ms. 71462 kilobits per second
1488765617 INFO Benchmark.c:62 Benchmark Switching in 4527ms. 45239 packets per second
```

### Optimized Build

#### Bench

```
$ Seccomp_NO=1 CFLAGS="-s -static -Wall -mfpu=neon-vfpv4 -mfloat-abi=hard -mcpu=cortex-a7 -mtune=cortex-a7 -fomit-frame-pointer -marm" ./do

# ./cjdroute --bench | grep "per second"
1488764572 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 8155ms. 147148 kilobits per second 
1488764576 INFO Benchmark.c:62 Benchmark Switching in 3963ms. 51678 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488764604 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 8159ms. 147076 kilobits per second 
1488764608 INFO Benchmark.c:62 Benchmark Switching in 3962ms. 51691 packets per second 
$ ./cjdroute --bench | grep "per second" 
1488764670 INFO Benchmark.c:62 Benchmark salsa20/poly1305 in 8155ms. 147148 kilobits per second
1488764674 INFO Benchmark.c:62 Benchmark Switching in 3987ms. 51366 packets per second
```

## TODO

* Better descriptions of platforms
* Explanation for each flag
* Further reading section, stuff like https://www.cryptopp.com/wiki/ARM_(Command_Line) and tomesh tests and gcc flag resources
