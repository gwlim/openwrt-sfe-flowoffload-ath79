# Fast Path Builds for OpenWRT

After flashing only basic services are enabled, to use additional features (including Flow Offload & SFE) you have to START them 
--------------------------------------------------------------------------------------------------------------------------------

Non-essential Kernel Modules have been moved to a tarball. Download and install them separately.
------------------------------------------------------------------------------------------------

Reboot recommended to ensure all routing passes through fast path
-----------------------------------------------------------------

List of Routers (Supported added base on suitability & request)
---------------------------------------------------------------

| Model | Actual Processor Architecture | Variant |
| --- | --- | --- |
| Mercury MW4530Rv1 | mips74k | Normal |
| Mikrotik RB450G | mips24k | Normal |
| Netgear WNDR3700v1 | mips24k | Normal |
| Netgear WNDR3700v4 | mips74k | Normal |
| Netgear WNDR3800CH | mips24k | Normal |
| Netgear WNDR4300v1 | mips74k | Normal |
| Phicomm K2T | mips74k | Normal |
| TP-Link Archer C5v1 | mips74k | Normal |
| TP-Link Archer C7v2-IL | mips74k | Normal |
| TP-Link Archer C7v2 | mips74k | Normal |
| TP-Link Archer C7v4 | mips74k | Normal |
| TP-Link Archer C7v5 | mips74k | Normal |
| TP-Link MR3420v1 | mips24k | Modded 64MB RAM |
| TP-Link MR3420v2 | mips74k | Modded 64MB RAM |
| TP-Link WDR3500v1 | mips74k | Normal |
| TP-Link WDR3600v1 | mips74k | Normal |
| TP-Link WDR4300v1 | mips74k | Normal |
| TP-Link WDR4310v1 | mips74k | Normal |
| TP-Link WDR4900v1 | mpc8548  | Normal |
| TP-Link WDR7500v3 | mips74k | Normal |
| TP-Link WR1043NDv1 | mips24k | Both |
| TP-Link WR1043NDv2 | mips74k | Normal |
| TP-Link WR1043NDv3 | mips74k | Normal |
| TP-Link WR1043NDv4 | mips74k | Normal |
| TP-Link WR1043Nv5 | mips74k | Normal |
| TP-Link WR2543ND | mips24k | Normal |
| Ubiquiti UniFi AC Lite| mips74k | Normal |
| Western Digital My Net N750| mips74k | Normal |


Summary
-------

This repository contains Openwrt firmware images compiled with shortcut-fe module, additional features, performance as well as security optimizations.

The firmware images are based on the Trunk Branch of Openwrt.

During to kernel configuration modifications, upstream kmods cannot be used but upstream packages can be installed

All the kmods are available in the tarball

If your router is among the supported (must be ar71xx, ath79 or mpc85xx based) and you want to use this firmware open an issue.

This firmware is optimize for networking performance aspect, I don't recommend running NAS on embedded devices.

For better performance NAS workloads should be running on dedicated devices.

Which to use ?
--------------

The images are subdivided into their respective SOC architectures.

There are differences between mips24k, mips74k, powerpc(mpc85xx) and different optimizations are required to maximise the performance respectively.

Linux Kernel size increase, as well as larger rootfs/security changes in Openwrt, 4MB Router devices are increasingly hard to support.

There are 2 factors affecting this:

-if the flash is too small, the firmware cannot be compiled to fit

-if the RAM is too small, after loading there is insufficient memory to load the firmware in memory and operate properly

Therefore if your router has 32MB RAM OR 4MB Flash please go for the mini builds, otherwise you can go for the normal builds.


Features
--------

Overclock firmwares for Atheros AR7161 is now available for Netgear WNDR3700v1-mips24k, Netgear WNDR3800CH-mips24k, Mikrotik RB450G-mips24k, look for the letters "oc" in the firmware filename. Overclocked settings CPU:800MHZ,RAM:400MHZ,AHB:200MHZ

Base on Linux Kernel 4.14 using GCC 7.4 toolchain (Both Builds)

Full IPv6 support (Both Builds)

PPPoE and L2TP Kmods included, xl2tpd can be installed from opkg (Both Builds)

Memory Operations optimization for mips24k,mips74k and mpc85xx Architectures (Both Builds)

Shortcut-fe Fast Path Module for accelerated NAT/Routing performance (Both Builds)

FlowOffload Fast Path Module for accelerated NAT/Routing performance (In Tarball Archives)

Default to BBR Congestion Control Algorithm (Both Builds)

```
root@openwrt:~# cat /proc/sys/net/ipv4/tcp_congestion_control
bbr
```

Stack-Smashing Protection (Normal Builds STRONG, Mini builds REGULAR)

SQM QoS (Normal Builds included, Mini Builds install from opkg)

Compiled with -O2 Compiler optimization (Normal Builds Only)

OpenSSL Crypto assembly optimizations for mips24k,mips74k and mpc85xx Architectures (Normal Builds Only)

Nlbwmon - Network Bandwidth Monitoring (Normal Builds Only)

4G LTE USB Support (Normal Builds Only, Drivers in Tarball Archive)

HTTPS-DNS-Proxy encrypted DNS to query upstream DNS Servers (Normal Builds Only)

https://hacks.mozilla.org/2018/05/a-cartoon-intro-to-dns-over-https/

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

https://openwrt.org/docs/guide-user/perf_and_log/benchmark.nat

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
