NAS – Shutdown Power Off and Power Up – CDOT Systems

Site will have Incident Ticket or Request Center ticket in out queue for servers and timing to shutdown: 

Bridge or IM contact will be set up at the time of shutdown to confirm all other hosts, Unix, ESX, Windows servers accessing the NAS have been shutdown. Once confirmed proceed with shutdown.

NAS – Shutdown Procedure

Login to the both nodes rm – SP (To check the complete halt status)

pthilipa@ito079596:~> ssh -l admin node01rm
admin@node01rm's password:
BMC node01> system console
Type Ctrl-D to exit.

SP-login: admin
Password:
*****************************************************
* This is an SP console session. Output from the    *
* serial console is also mirrored on this session.  *
*****************************************************
cap801::>


Node 2:
pthilipa@ito079596:~> ssh -l admin cap80102rm
admin@cap80102rm's password:
BMC cap80102> system console
Type Ctrl-D to exit.
SP-login: admin
Password:
*****************************************************
* This is an SP console session. Output from the    *
* serial console is also mirrored on this session.  *
*****************************************************
cap801::>

Trigger autosuppport:
cap801::> system node autosupport invoke -node * -type all -message "Planned_Shutdown" 
Run the below commands from the cluster shell:


pthilipa@ito079596:~> ssh -l 'fordap1\$pthilipa' cap801
Password:

cap801::> cluster ha modify -configured false
cap801::> storage failover modify –node node01 -enabled false
cap801::> storage failover modify –node cap80102 -enabled false
cap801::> halt -node node01 -inhibit-takeover true 
cap801::> halt -node cap80102 -inhibit-takeover true  (If getting error when halting this node ‘cap80102’, please run the below command)

cap801::> halt -node cap80102 -inhibit-takeover true -ignore-quorum-warnings true 




Once both nodes halted, please run the “system power off” from BMC/SP console.

BMC node01> system power off
BMC cap80102> system power off
