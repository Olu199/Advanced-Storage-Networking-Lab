
### NAT Configuration:
1. Enter configuration mode:
   ```bash
   configure
   ```

2. Set IP addresses for interfaces:
   ```bash
   set interfaces ethernet eth0 address 172.16.0.1/19
   set interfaces ethernet eth1 address 172.16.32.1/19
   set interfaces ethernet eth2 address 172.16.64.1/19
   ```

3. Configure static routes:
   ```bash
set protocols static route 0.0.0.0/0 next-hop 10.1.10.1
set protocols static route 172.16.96.0/19 next-hop 192.168.0.2
set protocols static route 172.16.128.0/19 next-hop 192.168.0.2
set protocols static route 172.16.160.0/19 next-hop 192.168.0.2
set protocols static route 172.16.192.0/19 next-hop 192.168.0.2
   ```

4. Set DNS servers:
   ```bash
   set system name-server 8.8.8.8
   set system name-server 8.8.4.4
   ```

5. Configure NAT for internet access:
   ```bash
   set nat source rule 100 description 'NAT for internet access for 172.16.0.0/19'
   set nat source rule 100 outbound-interface name eth3
   set nat source rule 100 source address 172.16.0.0/19
   set nat source rule 100 translation address masquerade

   set nat source rule 101 description 'NAT for internet access for 172.16.32.0/19'
   set nat source rule 101 outbound-interface name eth3
   set nat source rule 101 source address 172.16.32.0/19
   set nat source rule 101 translation address masquerade

   set nat source rule 102 description 'NAT for internet access for 172.16.64.0/19'
   set nat source rule 102 outbound-interface name eth3
   set nat source rule 102 source address 172.16.64.0/19
   set nat source rule 102 translation address masquerade
   ```

6. Commit, save, and exit:
   ```bash
   commit
   save
   exit
   ```

## Updated Configuration for Router Two (VyOS)

### NAT Configuration:
1. Enter configuration mode:
   ```bash
   configure
   ```

2. Set IP addresses for interfaces:
   ```bash
   set interfaces ethernet eth0 address 172.16.96.1/19
   set interfaces ethernet eth1 address 172.16.128.1/19
   set interfaces ethernet eth2 address 172.16.160.1/19
   set interfaces ethernet eth3 address 10.1.10.61/19
   set interfaces ethernet eth4 address 172.16.192.1/19
   ```

3. Configure static routes:
   ```bash
   set protocols static route 0.0.0.0/0 next-hop 10.1.10.1
   set protocols static route 172.16.0.0/19 next-hop 192.168.0.1
   set protocols static route 172.16.32.0/19 next-hop 192.168.0.1
   set protocols static route 172.16.64.0/19 next-hop 192.168.0.1

   ```

4. Set DNS servers:
   ```bash
   set system name-server 8.8.8.8
   set system name-server 8.8.4.4
   ```

5. Configure NAT for internet access:
   ```bash
   set nat source rule 100 description 'NAT for internet access for 172.16.96.0/19'
   set nat source rule 100 outbound-interface name eth3
   set nat source rule 100 source address 172.16.96.0/19
   set nat source rule 100 translation address masquerade

   set nat source rule 101 description 'NAT for internet access for 172.16.128.0/19'
   set nat source rule 101 outbound-interface name eth3
   set nat source rule 101 source address 172.16.128.0/19
   set nat source rule 101 translation address masquerade

   set nat source rule 102 description 'NAT for internet access for 172.16.160.0/19'
   set nat source rule 102 outbound-interface name eth3
   set nat source rule 102 source address 172.16.160.0/19
   set nat source rule 102 translation address masquerade

   set nat source rule 103 description 'NAT for internet access for 172.16.192.0/19'
   set nat source rule 103 outbound-interface name eth3
   set nat source rule 103 source address 172.16.192.0/19
   set nat source rule 103 translation address masquerade
   ```

6. Commit, save, and exit:
   ```bash
   commit
   save
   exit
   ```



Rename each router for easy identification:
```bash
configure
set system login user vyos-pve full-name 'Node1 Router2'
set system login user vyos-pve authentication plaintext-password 'new-password'
commit
save
```
Login using vyos-pve and the new-password. Then delete old user vyos
```bash
# configure
delete system login user vyos
commit
save
exit
```
