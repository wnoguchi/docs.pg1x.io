EIGRP
=============================

.. toctree::
   :maxdepth: 1
   :caption: Contents:

.. contents:: TOC

===========================
Foundation
===========================

* 高信頼性

  * Update
  * Query
  * Reply

* 無信頼性

  * Hello
  * Ack

Passive 利用可能状態
Active Query パケットを投げてルートを問合せ中

K値

#. 帯域幅 1
#. 負荷 0
#. 遅延 1
#. 信頼性 0
#. MTU 0

K5=0

Metric = (K1 x 帯域幅) + [(K2 x 帯域幅) / (256 - 負荷)] + (K3 x 遅延)

K5<>0

Metric = (K1 x 帯域幅) + [(K2 x 帯域幅) / (256 - 負荷)] + (K3 x 遅延) x [K5 / (信頼性 + K4)]

デフォルト

Metric = 帯域幅 + 遅延

Metric
IGRP 24 bit
EIGRP 32 bit

帯域幅 = (10^7 / リンクの最小帯域幅) x 256
遅延 = リンクの累積遅延 x 256

遅延は 10 usec 単位で計算する

DLY 20000 usec

だったら 2000 が遅延の値となる

マルチキャストアドレス 224.0.0.10

==================================
1-3 EIGRP の基本設定
==================================

EIGRP プロセスの起動
-----------------------------------------------------------

router eigrp <as-number>

ルータコンフィギュレーションモードに移行する
1～65535

EIGRP が動作するインターフェイスの定義
-----------------------------------------------------------

netowrk なんちゃら [ワイルドカードマスク]

帯域幅の指定
-------------------------

(config-if)#bandwidth <kbps>

V.35
X.25

学び

最近 Auto MDI/MDIX に慣れすぎててストレート同士で繋いでも大丈夫だと思ってたが、
C1841 同士だと違った

R1(config)#do sh ip int bri
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            172.16.1.1      YES manual up                    up
FastEthernet0/1            172.16.2.1      YES manual up                    up
Serial0/0/0                10.1.1.1        YES manual up                    up

クロスケーブルに変えたらリンクアップした。
L2 （データリンク層）レベルで繋がったということである。

R1(config)#do sh int fa0/1
FastEthernet0/1 is up, line protocol is up

R3(config-if)#do sh ip int bri
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down
FastEthernet0/1            172.16.2.3      YES manual up                    up
Serial0/0/0                unassigned      YES NVRAM  administratively down down

==================================================
1-4 EIGRP の検証
==================================================

EIGRP 検証の設定
-----------------------------

WAN リンクが無いのでスイッチで代用した。マルチアクセスネットワーク。

::

   R1>en
   R1#conf t
   Enter configuration commands, one per line.  End with CNTL/Z.
   R1(config)#int f0/0
   R1(config-if)#swi
   R1(config-if)#mo
   R1(config-if)#ip add
   R1(config-if)#ip address 10.1.10.1 255.255.255.0
   R1(config-if)#no shut
   R1(config-if)#
   *Feb 12 09:24:20.503: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
   R1(config-if)#int f0/1
   R1(config-if)#ip addr
   R1(config-if)#ip address 10.1.110.1 255.255.255.0
   R1(config-if)#no shut
   R1(config-if)#
   *Feb 12 09:29:18.647: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up
   R1(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.10.1       YES manual up                    up
   FastEthernet0/1            10.1.110.1      YES manual up                    up
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R1(config-if)#do ping 10.1.110.2
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.1.110.2, timeout is 2 seconds:
   .!!!!
   Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/4 ms
   R1(config-if)#do ping 10.1.110.3
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.1.110.3, timeout is 2 seconds:
   .!!!!
   Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/4 ms
   R1(config-if)#do ping 10.1.110.3
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.1.110.3, timeout is 2 seconds:
   !!!!!
   Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
   R1(config-if)#do ping 10.1.110.2
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.1.110.2, timeout is 2 seconds:
   !!!!!
   Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
   R1(config-if)#exit
   R1(config)#route
   R1(config)#router
   R1(config)#router eigrp 1
   R1(config-router)#network 10.0.0.0
   R1(config-router)#
   *Feb 12 09:32:01.151: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.2 (FastEthernet0/1) is up: new adjacency
   *Feb 12 09:32:01.727: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.3 (FastEthernet0/1) is up: new adjacency
   R1(config-router)#^Z
   R1#
   *Feb 12 09:32:05.955: %SYS-5-CONFIG_I: Configured from console by console

::

   R2>en
   R2#conf t
   Enter configuration commands, one per line.  End with CNTL/Z.
   R2(config)#int f0/0
   R2(config-if)#ip addre
   R2(config-if)#ip address 10.1.20.2 255.255.255.0
   R2(config-if)#no shut
   R2(config-if)#
   *Feb 12 10:26:03.631: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up
   *Feb 12 10:26:04.631: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
   R2(config-if)#
   *Feb 12 10:26:12.151: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to down
   R2(config-if)#sh ip int bri
                    ^
   % Invalid input detected at '^' marker.
   
   R2(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.2       YES manual up                    down
   FastEthernet0/1            unassigned      YES NVRAM  administratively down down
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R2(config-if)#
   *Feb 12 10:27:53.147: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
   R2(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.2       YES manual up                    up
   FastEthernet0/1            unassigned      YES NVRAM  administratively down down
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R2(config-if)#int f0/1
   R2(config-if)#ip address 10.1.110.2 255.255.255.0
   R2(config-if)#no shut
   R2(config-if)#
   *Feb 12 10:30:57.331: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up
   R2(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.2       YES manual up                    up
   FastEthernet0/1            10.1.110.2      YES manual up                    up
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R2(config-if)#router eigrp 1
   R2(config-router)#network 10.0.0.0
   R2(config-router)#
   *Feb 12 10:33:15.107: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.1 (FastEthernet0/1) is up: new adjacency
   *Feb 12 10:33:15.675: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.20.3 (FastEthernet0/0) is up: new adjacency
   *Feb 12 10:33:15.675: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.3 (FastEthernet0/1) is up: new adjacency
   R2(config-router)#^Z
   R2#
   *Feb 12 10:33:20.823: %SYS-5-CONFIG_I: Configured from console by console

::

   R3>en
   R3#conf t
   Enter configuration commands, one per line.  End with CNTL/Z.
   R3(config)#int f0/0
   R3(config-if)#ip address 10.1.20.3 255.255.255.0
   R3(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.3       YES manual administratively down down
   FastEthernet0/1            unassigned      YES NVRAM  administratively down down
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R3(config-if)#no shut
   R3(config-if)#
   *Feb 12 11:33:53.815: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
   R3(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.3       YES manual up                    up
   FastEthernet0/1            unassigned      YES NVRAM  administratively down down
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R3(config-if)#int f0/1
   R3(config-if)#ip address 10.1.110.3 255.255.255.0
   R3(config-if)#no shut
   R3(config-if)#
   *Feb 12 11:37:03.823: %LINK-3-UPDOWN: Interface FastEthernet0/1, changed state to up
   *Feb 12 11:37:04.823: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up
   R3(config-if)#do sh ip int bri
   Interface                  IP-Address      OK? Method Status                Protocol
   FastEthernet0/0            10.1.20.3       YES manual up                    up
   FastEthernet0/1            10.1.110.3      YES manual up                    up
   Serial0/0/0                unassigned      YES NVRAM  administratively down down
   R3(config-if)#router eigrp 1
   R3(config-router)#network 10.0.0.0
   R3(config-router)#
   *Feb 12 11:39:18.359: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.20.2 (FastEthernet0/0) is up: new adjacency
   *Feb 12 11:39:18.359: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.2 (FastEthernet0/1) is up: new adjacency
   *Feb 12 11:39:18.363: %DUAL-5-NBRCHANGE: EIGRP-IPv4 1: Neighbor 10.1.110.1 (FastEthernet0/1) is up: new adjacency
   R3(config-router)#^Z
   R3#
   *Feb 12 11:39:24.175: %SYS-5-CONFIG_I: Configured from console by console

EIGRP ネイバーの検証
-----------------------------

概要レベル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   sh ip eigrp neighbors

::

   R1#sh ip eigrp neighbors
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   1   10.1.110.3              Fa0/1             10 00:13:07    6   200  0  5
   0   10.1.110.2              Fa0/1             11 00:13:07    3   200  0  3
   
   R2#sh ip eigrp neighbors
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   2   10.1.110.3              Fa0/1             11 00:13:15    4   200  0  7
   1   10.1.20.3               Fa0/0             12 00:13:15    4   200  0  8
   0   10.1.110.1              Fa0/1             12 00:13:15    2   200  0  3
   
   R3#sh ip eigrp neighbors
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   2   10.1.110.1              Fa0/1             13 00:13:23    2   200  0  5
   1   10.1.110.2              Fa0/1             12 00:13:23 1020  5000  0  7
   0   10.1.20.2               Fa0/0             14 00:13:23 1593  5000  0  6

詳細表示
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   show ip eigrp neighbors detail

::

   R1#show ip eigrp neighbors detail
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   1   10.1.110.3              Fa0/1             10 00:36:51    6   200  0  5
      Version 6.0/3.0, Retrans: 0, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   0   10.1.110.2              Fa0/1             14 00:36:51    3   200  0  3
      Version 6.0/3.0, Retrans: 1, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   
   R2#show ip eigrp neighbors detail
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   2   10.1.110.3              Fa0/1             11 00:37:03    4   200  0  7
      Version 6.0/3.0, Retrans: 1, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   1   10.1.20.3               Fa0/0             12 00:37:03    4   200  0  8
      Version 6.0/3.0, Retrans: 1, Retries: 0, Prefixes: 2
      Topology-ids from peer - 0
   0   10.1.110.1              Fa0/1             14 00:37:04    2   200  0  3
      Version 6.0/3.0, Retrans: 0, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   
   R3#show ip eigrp neighbors detail
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   2   10.1.110.1              Fa0/1             14 00:37:26    2   200  0  5
      Version 6.0/3.0, Retrans: 0, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   1   10.1.110.2              Fa0/1             10 00:37:26 1020  5000  0  7
      Version 6.0/3.0, Retrans: 0, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   0   10.1.20.2               Fa0/0             11 00:37:26 1593  5000  0  6
      Version 6.0/3.0, Retrans: 1, Retries: 0, Prefixes: 2
      Topology-ids from peer - 0

短縮形
^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   R1#sh ip ei ne de
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   1   10.1.110.3              Fa0/1             14 00:47:07    6   200  0  5
      Version 6.0/3.0, Retrans: 0, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   0   10.1.110.2              Fa0/1             13 00:47:08    3   200  0  3
      Version 6.0/3.0, Retrans: 1, Retries: 0, Prefixes: 1
      Topology-ids from peer - 0
   R1#sh ip ei ne
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   1   10.1.110.3              Fa0/1             14 00:47:11    6   200  0  5
   0   10.1.110.2              Fa0/1             13 00:47:12    3   200  0  3
   R1#sh ip ei ne
   EIGRP-IPv4 Neighbors for AS(1)
   H   Address                 Interface       Hold Uptime   SRTT   RTO  Q  Seq
                                               (sec)         (ms)       Cnt Num
   1   10.1.110.3              Fa0/1             14 00:47:17    6   200  0  5
   0   10.1.110.2              Fa0/1             13 00:47:17    3   200  0  3

EIGRP ルートの検証
-----------------------------------------------------------

Advertised Distance(AD)

   R1#show ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
          + - replicated route, % - next hop override
   
   Gateway of last resort is not set
   
         10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
   C        10.1.10.0/24 is directly connected, FastEthernet0/0
   L        10.1.10.1/32 is directly connected, FastEthernet0/0
   D        10.1.20.0/24 [90/30720] via 10.1.110.3, 00:48:41, FastEthernet0/1
                         [90/30720] via 10.1.110.2, 00:48:41, FastEthernet0/1
   C        10.1.110.0/24 is directly connected, FastEthernet0/1
   L        10.1.110.1/32 is directly connected, FastEthernet0/1
   
   R2#sh ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
          + - replicated route, % - next hop override
   
   Gateway of last resort is not set
   
         10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
   D        10.1.10.0/24 [90/30720] via 10.1.110.1, 00:48:38, FastEthernet0/1
   C        10.1.20.0/24 is directly connected, FastEthernet0/0
   L        10.1.20.2/32 is directly connected, FastEthernet0/0
   C        10.1.110.0/24 is directly connected, FastEthernet0/1
   L        10.1.110.2/32 is directly connected, FastEthernet0/1
   
   R3#sh ip route
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
          + - replicated route, % - next hop override
   
   Gateway of last resort is not set
   
         10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
   D        10.1.10.0/24 [90/30720] via 10.1.110.1, 00:48:47, FastEthernet0/1
   C        10.1.20.0/24 is directly connected, FastEthernet0/0
   L        10.1.20.3/32 is directly connected, FastEthernet0/0
   C        10.1.110.0/24 is directly connected, FastEthernet0/1
   L        10.1.110.3/32 is directly connected, FastEthernet0/1

R1 については ``10.1.20.0/24`` に到達する FD のメトリックが同じなので等コストロードバランシングが行われている。

EIGRP のルートのみ表示したいときはケツに eigrp とつける。

   R1#sh ip route eigrp
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
          + - replicated route, % - next hop override
   
   Gateway of last resort is not set
   
         10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
   D        10.1.20.0/24 [90/30720] via 10.1.110.3, 01:02:13, FastEthernet0/1
                         [90/30720] via 10.1.110.2, 01:02:13, FastEthernet0/1

こいつも省略すると以下のような感じになる

   R1#sh ip ro ei
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
          D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
          N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
          E1 - OSPF external type 1, E2 - OSPF external type 2
          i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
          ia - IS-IS inter area, * - candidate default, U - per-user static route
          o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
          + - replicated route, % - next hop override
   
   Gateway of last resort is not set
   
         10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
   D        10.1.20.0/24 [90/30720] via 10.1.110.3, 01:03:02, FastEthernet0/1
                         [90/30720] via 10.1.110.2, 01:03:02, FastEthernet0/1

EIGRP 動作の検証
--------------------------

::

   R1#sh ip prot
   *** IP Routing is NSF aware ***
   
   Routing Protocol is "eigrp 1"
     Outgoing update filter list for all interfaces is not set
     Incoming update filter list for all interfaces is not set
     Default networks flagged in outgoing updates
     Default networks accepted from incoming updates
     EIGRP-IPv4 Protocol for AS(1)
       Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
       NSF-aware route hold timer is 240
       Router-ID: 10.1.110.1
       Topology : 0 (base)
         Active Timer: 3 min
         Distance: internal 90 external 170
         Maximum path: 4
         Maximum hopcount 100
         Maximum metric variance 1
   
     Automatic Summarization: disabled
     Maximum path: 4
     Routing for Networks:
       10.0.0.0
     Routing Information Sources:
       Gateway         Distance      Last Update
       10.1.110.3            90      01:07:41
       10.1.110.2            90      01:07:41
     Distance: internal 90 external 170
