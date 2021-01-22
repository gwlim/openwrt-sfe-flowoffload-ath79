# Fast Path Builds for OpenWRT

:exclamation:[PLEASE NOTE AR71XX DEVICES HAS BEEN DEPRECATED, PLEASE PORT THE DEVICES TO ATH79 BY MIGRATING THEM TO DTS CODE AT OPENWRT.ORG](https://forum.openwrt.org/t/porting-guide-ar71xx-to-ath79/13013)
------------------------------------------------------------------------------------------------------------------------------

[FAQ](https://github.com/gwlim/openwrt-sfe-flowoffload/wiki/FAQ)
---------

Known Issues
------------

32MB/4MB devices are no longer supported as it is too tedious and too much features have to be stripped to make it viable
Strongly recommend to upgrade to newer routers or buy Flash/DRAM chips to solder on and upgrade your units

Changelog
---------

```
Support ipq4019 and ramips-24kec, ramips-1004kc
Updated to kernel 5.4 for ath79
Fix up all unaligned memory accesses on ath79
Optimize SFE for IPv4 and IPv6
Fix up LuCI code for SFE
Fix up IPv6 SFE Offloading
Fixed up forgotten size optimization for kernel 5.4
Add support for modded flash expansion for ath79
Fix up port forwarding in SFE
```

After flashing only basic services are enabled, to use additional features (SFE) you have to START them 
-------------------------------------------------------------------------------------------------------

I can only test the builds on units I own, so for units I do not own please feedback if something works or not
--------------------------------------------------------------------------------------------------------------

Non-essential Kernel Modules have been moved to a tarball. Download and install them separately.
------------------------------------------------------------------------------------------------

Reboot recommended to ensure all routing passes through fast path
-----------------------------------------------------------------

List of Routers (Supported added base on suitability & request)
---------------------------------------------------------------

Supported routers are listed in the directories.
You can request for platforms already supported in openwrt
The following platforms are supported qca-ipq4019, ath79-mips24k, ramips-mips24kec, ramips-mips1004k, ath79-mips74k, freescale-mpc85xx

Summary
-------

This repository contains Openwrt firmware images compiled with shortcut-fe module, additional features, performance as well as security optimizations.

The firmware images are based on the Trunk Branch of Openwrt.

During to kernel configuration modifications, upstream kmods cannot be used but upstream packages can be installed

All the kmods are available in the tarball

If your router is among the supported (must be ath79 or mpc85xx based) and you want to use this firmware open an issue.

This firmware is optimize for networking performance aspect, I don't recommend running NAS on embedded devices.

For better performance NAS workloads should be running on dedicated devices.

Which to use ?
--------------

The images are subdivided into their respective SOC architectures.

There are differences between mips24k, mips24kec, mips74k, mips1004k, ipq4019, mpc85xx and different optimizations are required to maximise the performance respectively.

Linux Kernel size increase, as well as larger rootfs/security changes in Openwrt, 4MB Router devices are increasingly hard to support.

There are 2 factors affecting this:

-if the flash is too small, the firmware cannot be compiled to fit

-if the RAM is too small, after loading there is insufficient memory to load the firmware in memory and operate properly

Features
--------

Base on Linux Kernel 5.4 using GCC 8.4 toolchain

Full IPv6 support

PPPoE and L2TP Kmods included, xl2tpd can be installed from opkg

Memory Operations optimization for mips24k, mips24kec, mips1004kc, mips74k and mpc85xx Architectures (Both Builds)

Armv7 optimization for ipq4019

Shortcut-fe Fast Path Module for accelerated NAT/Routing performance

FlowOffload Fast Path Module for accelerated NAT/Routing performance

Default to CUBIC Congestion Control Algorithm 

```
root@openwrt:~# cat /proc/sys/net/ipv4/tcp_congestion_control
cubic
```

Stack-Smashing Protection (REGULAR)

SQM QoS (Normal Builds included, Mini Builds install from opkg)

Kernel Compiled with -O2 Compiler optimization (Normal Builds Only)

OpenSSL Crypto assembly optimizations for mips24k,mips74k and mpc85xx Architectures (Normal Builds Only)

Nlbwmon - Network Bandwidth Monitoring (Normal Builds Only)

4G LTE USB Support (Normal Builds Only, Drivers in Tarball Archive)

OpenVPN encrypted tunneling (Normal Builds Only)

Wireguard encrypted stateless tunneling (Normal Builds Only)

How to install from OEM firmware
--------------------------------

1. Download the **"factory"** firmware and upload it via the firmware upgrade page (Done for the first time only)

2. For Subsequent upgrades to newer Openwrt firmware, download the **"sysupgrade"** and upload it via the openwrt firmware upgrade page

3. [Use http://192.168.1.1 to access the OpenWrt Router Configuration Page](http://192.168.1.1)

4. Configure your Router settings and enable the features required

Use
---

Default Network Address after login: [http://192.168.1.1](http://192.168.1.1)

Subnet Mask: 255.255.255.0

Login: root

DYNACK is enabled on hostapd so for NETWORK -> WIRELESS -> ADVANCE SETTINGS -> DISTANCE OPTIMIZATION -> ENTER "auto" for best performance.

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/dynack.PNG)

To enable FlowOffload go to NETWORK -> FIREWALL

To enable SFE go to NETWORK -> FIREWALL

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/sfe-offload-1.PNG)

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/sfe-offload-2.PNG)

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/sfe-offload-3.PNG)

It is recommended to reboot after enabling it.

Running Benchmark
-----------------

To benchmark network performance

IPv4: https://openwrt.org/docs/guide-user/perf_and_log/benchmark.nat
IPv6: https://openwrt.org/docs/guide-user/perf_and_log/benchmark.ipv6.routing

The benchmark below is for WR1043NDv1 AR9132 SoC@400MHZ, anything better should easily hit wired speeds

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/bench.PNG)

To benchmark crypto

https://openwrt.org/docs/guide-user/perf_and_log/benchmark.openssl

Compared on the same hardware, this firmware should achieve faster crypto performance

If you need VPN performance, wireguard should perform significantly better than OpenVPN

Recovery in case of bad flash
-----------------------------

Most Routers have a tftp recovery mode to recover from bad flash.

The generic steps are highlighted below

https://openwrt.org/docs/guide-user/troubleshooting/tftpserver

However you will need to find the specific steps for your router model.

When running tftp server remember to set static ip on your computer and turn off your computer firewall temporarily.
