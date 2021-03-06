title: pktgen
date: 2016-03-18 12:00:30
toc: true
tags: [网络测试]
categories: networks
keywords: [发包, pktgen, linux, 网络测试, packet, generate, 内核, 高速, high spped, kernel]
description: 介绍 Linux 内核发包工具 pktgen 及其配置、使用方法和样例。
---

## 什么是 pktgen
pktgen 是一款 Linux 发包工具，可在内核高速发包。 

> Linux packet generator is a tool to generate packets at very high speed in the kernel.

## 使能 pktgen
内核配置文件开启 `CONFIG_NET_PKTGEN`，通过查看是否有 `/proc/net/pktgen/` 目录确认是否编译 OK。

## 使用 pktgen 发包

* 添加设备

```
echo "add_device eth3" > /proc/net/pktgen/kpktgend_0
```

* 配置报文
配置方法，样例仅给出部分配置
```
echo "pkt_size 64" > /proc/net/pktgen/eth3
echo "count 1000000" > /proc/net/pktgen/eth3
echo "dst_mac aa:bb:cc:dd:ee:ff" > /proc/net/pktgen/eth3
```
查看配置结果：
```
cat /proc/net/pktgen/eth3
```

* 发送报文

```
echo "start" > /proc/net/pktgen/pgctrl
```
查看发送结果：
```
cat /proc/net/pktgen/eth3
```

## 发包样例

```
[1209-20:29:14:517] root@ubuntu:/proc/net/pktgen# echo "pkt_size 1500" > /proc/net/pktgen/eth3   
[1209-20:29:26:980] root@ubuntu:/proc/net/pktgen# echo "count 1000000" > /proc/net/pktgen/eth3         
[1209-20:29:29:862] root@ubuntu:/proc/net/pktgen# echo "start" > /proc/net/pktgen/pgctrl      
[1209-20:29:48:407] root@ubuntu:/proc/net/pktgen# cat /proc/net/pktgen/eth3                   
[1209-20:29:48:430] Params: count 1000000  min_pkt_size: 1500  max_pkt_size: 1500
[1209-20:29:48:435]      frags: 0  delay: 0  clone_skb: 0  ifname: eth3
[1209-20:29:48:437]      flows: 0 flowlen: 0
[1209-20:29:48:441]      queue_map_min: 0  queue_map_max: 0
[1209-20:29:48:444]      dst_min:   dst_max: 
[1209-20:29:48:447]         src_min:   src_max: 
[1209-20:29:48:452]      src_mac: 02:10:18:2c:f8:0e dst_mac: 00:00:00:00:00:00
[1209-20:29:48:458]      udp_src_min: 9  udp_src_max: 9  udp_dst_min: 9  udp_dst_max: 9
[1209-20:29:48:460]      src_mac_count: 0  dst_mac_count: 0
[1209-20:29:48:464]      Flags: 
[1209-20:29:48:464] Current:
[1209-20:29:48:467]      pkts-sofar: 1000000  errors: 0
[1209-20:29:48:472]      started: 3219370012us  stopped: 3233111900us idle: 299798us
[1209-20:29:48:477]      seq_num: 1000001  cur_dst_mac_offset: 0  cur_src_mac_offset: 0
[1209-20:29:48:481]      cur_saddr: 192.168.2.1  cur_daddr: 0.0.0.0
[1209-20:29:48:486]      cur_udp_dst: 9  cur_udp_src: 9
[1209-20:29:48:490]      cur_queue_map: 0
[1209-20:29:48:491]      flows: 0
[1209-20:29:48:496] Result: OK: 13741888(c13442090+d299798) usec, 1000000 (1500byte,0frags)
[1209-20:29:48:499]   72770pps 873Mb/sec (873240000bps) errors: 0
[1209-20:30:05:839] root@ubuntu:/proc/net/pktgen# 
```

## 完整说明
> From [linuxfoundation.org pktgen](http://www.linuxfoundation.org/collaborate/workgroups/networking/pktgen)：
> ### Setup
> 
> Enable CONFIG_NET_PKTGEN to compile and build pktgen.o either in kernel or as module. Module is preferred. insmod pktgen if needed. Once running pktgen creates a thread on each CPU where each thread has affinty it's CPU. Monitoring and controlling is done via /proc. Easiest to select a suitable a sample script and configure.
>
> On a dual CPU:
>
```
 ps aux | grep pkt
 root       129  0.3  0.0     0    0 ?        SW    2003 523:20 [pktgen/0]
 root       130  0.3  0.0     0    0 ?        SW    2003 509:50 [pktgen/1]
```
> For montoring and control pktgen creates:
>
> * /proc/net/pktgen/pgctrl
> * /proc/net/pktgen/kpktgend_X
```
 # cat /proc/net/pktgen/kpktgend_0 
 Name: kpktgend_0  max_before_softirq: 10000
 Running: 
 Stopped: eth1 
 Result: OK: max_before_softirq=10000
```
> Most important the devices assigned to thread. Note! A device can only belong to one thread.
> */proc/net/pktgen/ethX
> Param section holds configured info. Current hold running stats. Result is printed after run or after interruption. Example:
```
 # cat /proc/net/pktgen/eth1
 Params: count 10000000  min_pkt_size: 60  max_pkt_size: 60
      frags: 0  delay: 0  clone_skb: 1000000  ifname: eth1
      flows: 0 flowlen: 0
      dst_min: 10.10.11.2  dst_max: 
      src_min:   src_max: 
      src_mac: 00:00:00:00:00:00  dst_mac: 00:04:23:AC:FD:82
      udp_src_min: 9  udp_src_max: 9  udp_dst_min: 9  udp_dst_max: 9
      src_mac_count: 0  dst_mac_count: 0 
      Flags: 
 Current:
      pkts-sofar: 10000000  errors: 39664
      started: 1103053986245187us  stopped: 1103053999346329us idle: 880401us
      seq_num: 10000011  cur_dst_mac_offset: 0  cur_src_mac_offset: 0
      cur_saddr: 0x10a0a0a  cur_daddr: 0x20b0a0a
      cur_udp_dst: 9  cur_udp_src: 9
      flows: 0
 Result: OK: 13101142(c12220741+d880401) usec, 10000000 (60byte,0frags)
   763292pps 390Mb/sec (390805504bps) errors: 39664
```
> 
> #### Confguring
> This is done via the /proc interface easiest done via pgset in the scripts
>
> Examples:
```
 pgset "clone_skb 1"     sets the number of copies of the same packet
 pgset "clone_skb 0"     use single SKB for all transmits
 pgset "pkt_size 9014"   sets packet size to 9014
 pgset "frags 5"         packet will consist of 5 fragments
 pgset "count 200000"    sets number of packets to send, set to zero
                        for continious sends untill explicitl stopped.
 pgset "delay 5000"      adds delay to hard_start_xmit(). nanoseconds
 pgset "dst 10.0.0.1"    sets IP destination addres
                        (BEWARE! This generator is very aggressive!)
 pgset "dst_min 10.0.0.1"            Same as dst
 pgset "dst_max 10.0.0.254"          Set the maximum destination IP.
 pgset "src_min 10.0.0.1"            Set the minimum (or only) source IP.
 pgset "src_max 10.0.0.254"          Set the maximum source IP.
 pgset "dst6 fec0::1"     IPV6 destination address
 pgset "src6 fec0::2"     IPV6 source address
 pgset "dstmac 00:00:00:00:00:00"    sets MAC destination address
 pgset "srcmac 00:00:00:00:00:00"    sets MAC source address
 pgset "src_mac_count 1" Sets the number of MACs we'll range through.  
                        The 'minimum' MAC is what you set with srcmac.
 pgset "dst_mac_count 1" Sets the number of MACs we'll range through.
                        The 'minimum' MAC is what you set with dstmac.
 pgset "flag [name]"     Set a flag to determine behaviour.  Current flags
                        are: IPSRC_RND #IP Source is random (between min/max),
                             IPDST_RND, UDPSRC_RND,
                             UDPDST_RND, MACSRC_RND, MACDST_RND 
  pgset "udp_src_min 9"   set UDP source port min, If < udp_src_max, then
                        cycle through the port range.
  pgset "udp_src_max 9"   set UDP source port max.
  pgset "udp_dst_min 9"   set UDP destination port min, If < udp_dst_max, then
                        cycle through the port range.
  pgset "udp_dst_max 9"   set UDP destination port max.
  pgset stop    	          aborts injection. Also, ^C aborts generator.
```
> 
> ### Examples
> 
> A collection of small tutorial scripts for pktgen is in expamples dir.
> * pktgen.conf-1-1 # 1 CPU 1 dev
> * pktgen.conf-1-2 # 1 CPU 2 dev
> * pktgen.conf-2-1 # 2 CPU's 1 dev
> * pktgen.conf-2-2 # 2 CPU's 2 dev
> * pktgen.conf-1-1-rdos # 1 CPU 1 dev w. route DoS
> * pktgen.conf-1-1-ip6 # 1 CPU 1 dev ipv6
> * pktgen.conf-1-1-ip6-rdos # 1 CPU 1 dev ipv6 w. route DoS
> * pktgen.conf-1-1-flows # 1 CPU 1 dev multiple flows.
> Run in shell: `./pktgen.conf-X-Y` It does all the setup including sending.
> 
> #### Interrupt affinity
> 
> Note when adding devices to a specific CPU there good idea to also assign `/proc/irq/XX/smp_affinity` so the TX-interrupts gets bound to the same CPU. as this reduces cache bouncing when freeing skb's.
> 
> ### Commands
> 
> Pgcontrol commands:
> * start
> * stop
> Thread commands:
> * add_device
> * rem_device_all
> * max_before_softirq
> Device commands:
> * count
> * clone_skb
> * debug
> * frags
> * delay
> * src_mac_count
> * dst_mac_count
> * pkt_size
> * min_pkt_size
> * max_pkt_size
> * udp_src_min
> * udp_src_max
> * udp_dst_min
> * udp_dst_max
> * flag
>   + IPSRC_RND
>   + TXSIZE_RND
>   + IPDST_RND
>   + UDPSRC_RND
>   + UDPDST_RND
>   + MACSRC_RND
>   + MACDST_RND
> * dst_min
> * dst_max
> * src_min
> * src_max
> * dst_mac
> * src_mac
> * clear_counters
> * dst6
> * src6
> * flows
> * flowlen
