CCNA v3.0 R&S Cheat Sheet
====================================

**ABSOLUTELY NO WARRANTY!!**

Back to :doc:`/tech/network/cisco/certification/ccna/rs/v3.0/index`

.. toctree::
   :maxdepth: 1
   :caption: Contents:

.. contents:: TOC

========================
チラ裏
========================

#. ネットワークアドレスの計算はネットワークの重複に気をつけよ
#. auto-summary はネットワークアドレスを集約してアドバタイズするので Loopback アドレスの /32 ビットルートが載らないことがある
#. 不連続サブネットでは自動集約を有効にすると通信できない（ ルーティングテーブルにないものは Null0 インタフェースに送られパケットが破棄される ）
#. DR, BDR にかかわらず OSPF ルーターはルーティングテーブルをエリア内すべてのルーティングテーブルを保持する
#. RSTP には Listening 状態はない

===================================
IEEE 規格と RFC 規格の違い
===================================

IEEE 規格
  物理レイヤー L1, L2 での規格に関するもの。
RFC 規格
  L3 以上の規格が規定される。

=============================
ICND2
=============================

STP
-----------------------------------

::

   spanning-tree vlan vlan-id root {primary | secondary}

::

   spanning-tree vlan 10 root primary
   
   24576 = 0b 0110 0000 0000 0000

::

   spanning-tree vlan 10 root secondary
   
   28672 = 0b 0111 0000 0000 0000

デフォルトのプライオリティ::

   32768 = 0b 1000 0000 0000 0000

==================================
IEEE Table
==================================

+-------------------------------------------+----------------------+
| Name                                      | IEEE                 |
+===========================================+======================+
| STP                                       | IEEE 802.1d          |
+-------------------------------------------+----------------------+
| RSTP                                      | IEEE 802.1w          |
+-------------------------------------------+----------------------+
| MST                                       | IEEE 802.1s          |
+-------------------------------------------+----------------------+
| LACP(Link Aggregation Control Protocol)   | IEEE 802.3ad         |
+-------------------------------------------+----------------------+

==================================
Administrative Distance
==================================

+--------------------------------+---------------+
| Route Information              | AD Value      |
+================================+===============+
| Directly Connected             | 0             |
+--------------------------------+---------------+
| Static Route                   | 1             |
+--------------------------------+---------------+
| EIGRP                          | 90            |
+--------------------------------+---------------+
| OSPF                           | 110           |
+--------------------------------+---------------+
| RIP                            | 120           |
+--------------------------------+---------------+
| Unknown                        | 255           |
+--------------------------------+---------------+

===============
Misc
===============

回線スピード
------------------------------------------------

+----------------------------------+----------------------+
| Name                             | Value                |
+==================================+======================+
| T1                               | 1.544Mbps(1544Kbps)  |
+----------------------------------+----------------------+
| T3                               | 44.736Mbps           |
+----------------------------------+----------------------+

用語
-----------

* Application Policy Infrastructure Controller(APIC)
* APIC-EM (Application Policy Infrastructure Controller Enterprise Module)
* Cisco ACI (Application Centric Infrastructure)
* DSLAM（Digital Subscriber Line Access Multiplexer）
* SPAN (Switched Port Analyzer)
* Flexible NetFlow ネットワークの解析
* LLQ (Low-Latency Queueing)

ルーターの名称でよく使われる略称
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* HQ (Headquarters) Office 本社のこと
* BO (Branch Office)
* CE (Customer Edge Router)
* PE (Provider Edge Router)

KB
--------

デフォルトルート
^^^^^^^^^^^^^^^^^^^^^

ルータに設定するのは「デフォルトゲートウェイ」では無い
ルータに設定するのは基本的に「デフォルトルート」。

ループバックインタフェース
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ループバックインタフェースはデフォルトでアクティブなので ``no shutdown`` は不要。

QoS で制御できるもの
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* ジッタ(Jitter)
* 遅延(Delay)
* 損失(Loss)
* 帯域幅(Bandwidth)

なお、 *負荷* については制御できない。

PPPoE
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* アクティブディスカバリフェーズ

  * PPPoE サーバの探索
  * セッション ID の割り当て

* セッションフェーズ

OSPF Cost
^^^^^^^^^^^^^^^^^^^^

100Mbps / 256Kbps = 390

100Mbps = 10^8

HSRP
----------------

#. Initial
#. Learn
#. Listen
#. Speak
#. Standby
#. Active

======================
要復習
======================

* QoS DSCP 6bit IP in TOS (Type of Service)
* GRE トンネル
* CoS (Class of Service) 3bit Ethernet
* MPLS (Multi-Protocol Label Switching)
* IP SLA

=====================
Commands
=====================

デバッグメッセージの表示をタイムスタンプとする::

   service timestamps debug

trunk check::

   show interfaces trunk
   show interfaces status
   show interfaces switchport
   show vlan

BGP
----------

BGPルータ ID 確認::

   show ip bgp

AS番号確認::

   show ip bgp summary

QoS
---------------

#. ToS(Type of Service) L3
#. CoS(Class of Service) L2

==========================================
IP Protocol Number
==========================================

* 47 GRE
* 6 TCP
