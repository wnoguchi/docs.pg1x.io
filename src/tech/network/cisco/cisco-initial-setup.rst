Cisco Initial Setup
===================================

.. contents:: TOC

=============================
Configure
=============================

Router
-----------------

::

   en
   conf t
   no service config
   line console 0
   exec-timeout 0 0
   logging synchronous
   exit
   hostname R1
   int fa0/1
   ip addr 172.17.1.5 255.255.255.0
   no shut
   exit
   ip route 0.0.0.0 0.0.0.0 172.17.1.254
   enable secret password
   line vty 0 15
   exec-timeout 0 0
   password password
   login
   end
   write

Catalyst
------------------------

::

   en
   conf t
   no service config
   line console 0
   exec-timeout 0 0
   logging synchronous
   exit
   hostname SW4
   int vlan 1
   ip addr 172.17.1.5 255.255.255.0
   no shut
   exit
   ip default-gateway 172.16.1.1
   enable secret password
   line vty 0 15
   exec-timeout 0 0
   password password
   login
   end
   write


===============================
Clear Setting
===============================

Router
------------------------

::

   enable
   delete nvram:startup-config
   reload

Catalyst
------------------------

::

   enable
   delete flash:vlan.dat
   delete nvram:startup-config
   reload

==============
Tips
==============

初期化するたびにセットアップするか聞かれてうざい
------------------------------------------------------------------------

::

   % Please answer 'yes' or 'no'.
   Would you like to enter the initial configuration dialog? [yes/no]: no
