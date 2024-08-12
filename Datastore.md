














vserver1/vserver1_v0001/q00001 = 10752GB , ltp1_1_plant1_01
export-policy create -vserver vserver1 -policyname vserver1_v0001
export-policy create -vserver vserver1 -policyname vserver1_v0001_q00001
export-policy rule create -vserver vserver1 -policyname vserver1_v0001 -ruleindex 1 -protocol any -clientmatch 19.70.67.4      -rorule any -rwrule none -anon 65534 -superuser none -allow-suid true -allow-dev true
export-policy rule create -vserver vserver1 -policyname vserver1_v0001 -ruleindex 2 -protocol any -clientmatch 19.116.3.144/28 -rorule any -rwrule none -anon 65534 -superuser none -allow-suid true -allow-dev true
export-policy rule create -vserver vserver1 -policyname vserver1_v0001_q00001 -ruleindex 1 -protocol nfs -clientmatch 19.70.67.4      -rorule sys -rwrule sys -anon 65534 -superuser sys -allow-suid true -allow-dev true
export-policy rule create -vserver vserver1 -policyname vserver1_v0001_q00001 -ruleindex 2 -protocol nfs -clientmatch 19.116.3.144/28 -rorule sys -rwrule sys -anon 65534 -superuser sys -allow-suid true -allow-dev true
vol create -vserver vserver1 -volume vserver1_v0001 -aggregate ltp80104_sata_aggr1 -max-autosize 10752GB -size  2688GB   -junction-path /vserver1_v0001 -state online -type RW -policy vserver1_v0001 -space-guarantee none -snapshot-policy vmware -autosize-mode grow_shrink -percent-snapshot-space 0 -qos-adaptive-policy-group kfs-premium-vserver1 -security-style unix -comment ltp1_1_plant1_01      
vol efficiency on -vserver vserver1 -volume vserver1_v0001
vol efficiency modify -vserver vserver1 -volume vserver1_v0001 -schedule auto@10
qtree create -vserver vserver1 -volume vserver1_v0001 -qtree q00001 -security-style unix -oplock-mode enable -export-policy vserver1_v0001_q00001
volume quota policy rule create -vserver vserver1 -policy-name default -volume vserver1_v0001 -type tree -target q00001 -disk-limit 10752GB
vol quota on -vserver vserver1 -volume vserver1_v0001



export-policy rule create -vserver vserver1 -policyname vserver1_vmware_vol -ruleindex 1 -protocol any -clientmatch 19.39.128.128/26 -rorule any -rwrule none -anon 65534 -superuser sys -allow-suid true -allow-dev true
export-policy rule create -vserver vserver1 -policyname vserver1_vmware_vol_qtree -ruleindex 1 -protocol nfs -clientmatch 19.39.128.128/26 -rorule sys -rwrule sys -anon 65534 -superuser sys -allow-suid true -allow-dev true