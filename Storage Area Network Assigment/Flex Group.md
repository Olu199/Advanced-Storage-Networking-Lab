```bash
vserver create -vserver WinA_ontap7_f_01 -rootvolume WinA_ontap7_f_01_root 
vserver create -vserver WinA_ontap7_f_02 -rootvolume WinA_ontap7_f_02_root 
vserver create -vserver WinA_ontap7_f_03 -rootvolume WinA_ontap7_f_03_root 
vserver create -vserver WinA_ontap7_f_04 -rootvolume WinA_ontap7_f_04_root 
```

```bash
vol create -auto-provision-as flexgroup -vserver WinA_ontap7_f_01 -volume WinA_ontap7_f_01_vol01 -support-tiering false -use-mirrored-aggregates false -state online -policy default -unix-permissions ---rwxr-xr-x -type RW -snapshot-policy default -foreground true -tiering-policy none -size 2gb
```
