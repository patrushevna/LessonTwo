# LessonTwo
[root@testraid ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda      8:0    0   40G  0 disk  
└─sda1   8:1    0   40G  0 part  /
sdb      8:16   0  250M  0 disk  
└─md0    9:0    0  744M  0 raid6 
sdc      8:32   0  250M  0 disk  
└─md0    9:0    0  744M  0 raid6 
sdd      8:48   0  250M  0 disk  
└─md0    9:0    0  744M  0 raid6 
sde      8:64   0  250M  0 disk  
└─md0    9:0    0  744M  0 raid6 
sdf      8:80   0  250M  0 disk  
└─md0    9:0    0  744M  0 raid6 
[root@testraid ~]# cat /proc/mdstat 
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid6 sdf[4] sde[3] sdd[2] sdc[1] sdb[0]
      761856 blocks super 1.2 level 6, 512k chunk, algorithm 2 [5/5] [UUUUU]
      
unused devices: <none>
[root@testraid ~]# mdadm /dev/md0 --fail /dev/sdc
mdadm: set /dev/sdc faulty in /dev/md0
[root@testraid ~]# cat /proc/mdstat 
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid6 sdf[4] sde[3] sdd[2] sdc[1](F) sdb[0]
      761856 blocks super 1.2 level 6, 512k chunk, algorithm 2 [5/4] [U_UUU]
      
unused devices: <none>
[root@testraid ~]# mdadm /dev/md0 --remove /dev/sdc
mdadm: hot removed /dev/sdc from /dev/md0
[root@testraid ~]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Tue Nov 26 06:30:15 2019
        Raid Level : raid6
        Array Size : 761856 (744.00 MiB 780.14 MB)
     Used Dev Size : 253952 (248.00 MiB 260.05 MB)
      Raid Devices : 5
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Tue Nov 26 06:34:53 2019
             State : clean, degraded 
    Active Devices : 4
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : testraid:0  (local to host testraid)
              UUID : a62d24cb:6af8eb15:ab7a1b99:1eaab30d
            Events : 20

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       -       0        0        1      removed
       2       8       48        2      active sync   /dev/sdd
       3       8       64        3      active sync   /dev/sde
       4       8       80        4      active sync   /dev/sdf
[root@testraid ~]# mdadm /dev/md0 --add /dev/sdc
mdadm: added /dev/sdc
[root@testraid ~]# cat /proc/mdstat 
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid6 sdc[5] sdf[4] sde[3] sdd[2] sdb[0]
      761856 blocks super 1.2 level 6, 512k chunk, algorithm 2 [5/5] [UUUUU]
      
unused devices: <none>
[root@testraid ~]# 
