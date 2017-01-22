Switch Misc. Topics
==============================================

====================
XMODEM
====================

::

   Switch#erase flash:
   Erasing the flash filesystem will remove all files! Continue? [confirm]
   
   Using driver version 1 for media type 1
   Base ethernet MAC Address: 00:22:bd:89:21:80
   Xmodem file system is available.
   The password-recovery mechanism is enabled.
   Initializing Flash...
   mifs[2]: 0 files, 1 directories
   mifs[2]: Total bytes     :    3870720
   mifs[2]: Bytes used      :       1024
   mifs[2]: Bytes available :    3869696
   mifs[2]: mifs fsck took 0 seconds.
   mifs[3]: 0 files, 1 directories
   mifs[3]: Total bytes     :   27998208
   mifs[3]: Bytes used      :       1024
   mifs[3]: Bytes available :   27997184
   mifs[3]: mifs fsck took 0 seconds.
   ...done Initializing Flash.
   done.
   Loading "flash:c2960-lanbasek9-mz.150-2.SE8/c2960-lanbasek9-mz.150-2.SE8.bin"...flash:c2960-lanbasek9-mz.150-2.SE8/c2960-lanbasek9-mz.150-2.SE8.bin: no such file or directory
   
   Error loading "flash:c2960-lanbasek9-mz.150-2.SE8/c2960-lanbasek9-mz.150-2.SE8.bin"
   
   Interrupt within 5 seconds to abort boot process.
   Boot process failed...
   
   The system is unable to boot automatically.  The BOOT
   environment variable needs to be set to a bootable
   image.
   
   
   switch:

* `Ciscoデバイスの管理 - Catalystスイッチ - xmodemによるIOSのダウンロード <http://www.infraexpert.com/study/ciscoios14.html>`_

::

   copy xmodem:c2960-lanbasek9-mz.150-2.SE8.bin flash:
   copy xmodem: flash:c2960-lanbasek9-mz.150-2.SE8.bin

====================================================================
config の特定インタフェースの設定を見る
====================================================================

::

   CSW1#sh run int fa 0/11
   Building configuration...
   
   Current configuration : 141 bytes
   !
   interface FastEthernet0/11
    switchport access vlan 10
    switchport mode access
    spanning-tree portfast
    spanning-tree bpduguard enable
   end

