node01

volume show my_volume -fields volume-style-extended,state
set -privilege advanced
volume conversion start -vserver vs1 -volume flexvol -check-only true
volume conversion start -vserver svm_name -volume vol_name
volume show vol_name -fields -volume-style-extended,state

ecc9010106.hosts.cloud.ford.com/vol1/vol1



vol quota policy rule  show  -vserver  vol1 -policy-name vol1 -volume vol1

node01::> vol quota policy rule show -vserver vserver-policy-name default -volume vol1,vol2,vol3

Vserver: vserver       Policy: default           Volume: vol1

                                               Soft             Soft
                         User         Disk     Disk   Files    Files
Type   Target    Qtree   Mapping     Limit    Limit   Limit    Limit  Threshold
-----  --------  ------- -------  --------  -------  ------  -------  ---------
tree   vol1  "" -  83496GB  34026GB       -        -          -

Vserver: vserver       Policy: default           Volume: vol2

                                               Soft             Soft
                         User         Disk     Disk   Files    Files
Type   Target    Qtree   Mapping     Limit    Limit   Limit    Limit  Threshold
-----  --------  ------- -------  --------  -------  ------  -------  ---------
tree   vol2  "" -  88616GB  38122GB       -        -          -

Vserver: vserver       Policy: default           Volume: vol3

                                               Soft             Soft
                         User         Disk     Disk   Files    Files
Type   Target    Qtree   Mapping     Limit    Limit   Limit    Limit  Threshold
-----  --------  ------- -------  --------  -------  ------  -------  ---------
tree   vol3  "" -  99880GB  47134GB       -        -          -
3 entries were displayed.

node01::> vol show -vserver vserver-volume vol1,vol2,vol3
Vserver   Volume       Aggregate    State      Type       Size  Available Used%
--------- ------------ ------------ ---------- ---- ---------- ---------- -----
vservervol1 -   online     RW     163840GB    77652GB   40%
vservervol2 -   online     RW     204799GB    94178GB   42%
vservervol3 -   online     RW     204799GB    93732GB   42%
3 entries were displayed.





volume quota policy rule modify -vserver vserver -policy-name default -volume vol1 -type tree -target vol1 -disk-limit 93736GB -qtree ""
volume quota policy rule modify -vserver vserver -policy-name default -volume vol2 -type tree -target vol2 -disk-limit 98856GB -qtree ""
volume quota policy rule modify -vserver vserver -policy-name default -volume vol3 -type tree -target vol3 -disk-limit 110120GB -qtree ""

volume quota resize -volume cct8101bu01_cct1_1 -vserver cct8101bu01 -foreground