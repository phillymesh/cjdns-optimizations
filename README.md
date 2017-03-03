# README

This document was created to track the performance of CJDNS builds on various platforms, with and without compilation flags. Each build utilizes gcc, while the CJDNS version is noted in each case.

Note: I am by no means an optimation update. If anything is incorrect or could be done better, feel free to open an issue or pull request.

## Orange Pi Zero

Both builds were performed on Armbian, Armbian_5.25_Orangepizero_Debian_jessie_default_3.4.113.img using cjdns-v19.1

```
$ uname -a
Linux orangepizero 3.4.113-sun8i #10 SMP PREEMPT Thu Feb 23 19:55:00 CET 2017 armv7l GNU/Linux
```

**Default Build**

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

**Optimized Build**

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

## Pine A64

Both builds were performed on Armbian, Armbian_5.25_Pine64_Debian_jessie_default_3.10.104.img using cjdns-v19.1

``` 
$ uname -a
Linux pine64 3.10.104-pine64 #15 SMP PREEMPT Fri Feb 3 18:12:41 CET 2017 aarch64 GNU/Linux
```

**Default Build**

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

**Optimized Build**

The Pine A64 is advertised to have NEON support and hard-float, but hcc complains if they are specified

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

## BeagleBone Black

Both builds were performed on Debian IoT, the official headless Debian for BeagleBone, bone-debian-8.6-iot-armhf-2016-12-09-4gb.img using cjdns-v19.1

``` 
$ uname -a
Linux beaglebone 4.4.36-ti-r72 #1 SMP Wed Dec 7 22:29:53 UTC 2016 armv7l GNU/Linux
```

**Default Build**

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

**Optimized Build**

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

## TODO

* Better descriptions of platforms
* Explanation for each flag
* Further reading section, stuff like https://www.cryptopp.com/wiki/ARM_(Command_Line) and tomesh tests and gcc flag resources