volume create -vserver SAN -volume thick -size 500mb -aggregate aggr3_ontap7_01_fcal -space-slo thick
lun create -vserver SAN -volume thick -lun thick -size 400mb -ostype windows_2008 -space-reserve enabled

volume show -vserver SAN -volume thick -field space-guarantee,fractional-reserve,autosize-mode,percent-snapshot-space

volume snapshot autodelete show -vserver SAN -volume thick


volume create -vserver SAN -volume semi_thick -size 500mb -aggregate aggr3_ontap7_01_fcal -space-slo semi_thick
lun create -vserver SAN -volume semi_thick -lun semi_thick -size 400mb -ostype windows_2008 -space-reserve enabled

volume show -vserver SAN -volume semi_thick -field space-guarantee,fractional-reserve,autosize-mode,percent-snapshot-space

volume snapshot autodelete show -vserver SAN -volume semi_thick


volume create -vserver SAN -volume none -size 500mb -aggregate aggr3_ontap7_01_fcal -space-slo none
lun create -vserver SAN -volume none -lun none -size 400mb -ostype windows_2008 -space-reserve enabled

volume show -vserver SAN -volume none -field space-guarantee,fractional-reserve,autosize-mode,percent-snapshot-space

volume snapshot autodelete show -vserver SAN -volume none
