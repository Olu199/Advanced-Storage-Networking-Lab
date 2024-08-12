
volume size:
vol show -vserver vserver -volume vserver_cct1_1 -fields autosize-mode, aggregate, aggr-list,size,max-autosize

vserver     volume             aggregate           aggr-list           size    max-autosize autosize-mode
----------- ------------------ ------------------- ------------------- ------- ------------ -------------
vserver vserver_cct1_1 ccth10101_sas_aggr1 ccth10101_sas_aggr1 10752GB 12902GB      off



vol show -vserver vserver -volume vserver_cct1_1 -fields size, available , autosize-mode, used

vserver     volume             size    available used   autosize
----------- ------------------ ------- --------- ------ --------
vserver vserver_cct1_1 10752GB 6841GB    1760GB off

aggregate size:
aggr show -aggregate ccth10101_sas_aggr1 -fields size, availsize, usedsize

aggregate           availsize size    usedsize
------------------- --------- ------- --------
ccth10101_sas_aggr1 33601GB   39590GB 5988GB

Qtree size show:
qtree status -v vserver_cct1_1

Vserver    Volume        Qtree        Style        Oplocks   Status
---------- ------------- ------------ ------------ --------- --------
vserver vserver_cct1_1 ""     unix         enable    normal
vserver vserver_cct1_1 plant1_01 unix      enable    normal
vserver vserver_cct1_1 plant1_02 unix      enable    normal
vserver vserver_cct1_1 plant1_03 unix      enable    normal
4 entries were displayed.

quota report -vserver vserver -volume vserver_cct1_1
                                    ----Disk----  ----Files-----   Quota
Volume   Tree      Type    ID        Used  Limit    Used   Limit   Specifier
-------  --------  ------  -------  -----  -----  ------  ------   ---------
vserver_cct1_1
         plant1_01
                   tree    1
                                   2583GB
                                          3000GB     241       -   plant1_01
vserver_cct1_1
         plant1_02
                   tree    2
                                   2488GB
                                          3000GB     338       -   plant1_02
vserver_cct1_1
         plant1_03
                   tree    3
                                   2928GB
                                          3696GB     551       -   plant1_03
3 entries were displayed.


Volume Resize: *10752 + 3750* to GB
volume modify -vserver vserver -volume vserver_cct1_1 -size 14502GB

Quota limit modify:
vserver show -vserver vserver  -fields quota-policy 

vserver     quota-policy
----------- ------------
vserver vserver

vol quota policy rule  show  -vserver  vserver -policy-name vserver -volume vserver_cct1_1

                                               Soft             Soft
                         User         Disk     Disk   Files    Files
Type   Target    Qtree   Mapping     Limit    Limit   Limit    Limit  Threshold
-----  --------  ------- -------  --------  -------  ------  -------  ---------
tree   plant1_01
                 ""      -          3000GB        -       -        -          -
tree   plant1_02
                 ""      -          3000GB        -       -        -          -
tree   plant1_03
                 ""      -          3696GB        -       -        -          -
3 entries were displayed.

volume quota policy rule show -type  tree  -target plant1_02 -volume vserver_cct1_1
Vserver: vserver       Policy: vserver       Volume: vserver_cct1_1

                                               Soft             Soft
                         User         Disk     Disk   Files    Files
Type   Target    Qtree   Mapping     Limit    Limit   Limit    Limit  Threshold
-----  --------  ------- -------  --------  -------  ------  -------  ---------
tree   plant1_02
                 ""      -          3000GB        -       -        -          -
				 

volume quota policy rule modify -vserver vserver -policy-name vserver -volume vserver_cct1_1 -type tree -target plant1_02 -disk-limit 5048GB -qtree ""


volume quota resize -volume vserver_cct1_1 -vserver vserver -foreground

volume quota report
