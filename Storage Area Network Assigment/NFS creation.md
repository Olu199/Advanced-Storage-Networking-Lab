# NetApp ONTAP NFS Setup Guide

## Overview
This guide provides a step-by-step process for setting up and configuring NFS on a NetApp ONTAP system, along with instructions for mounting NFS shares on a Linux client.

## Step 1: Create Vserver and NFS Configuration

1. **Create the Vserver**:
   ```bash
   vserver create -vserver nfs -rootvolume nfs_root -rootvolume-security-style unix
   ```
2. **Enable NFS on the Vserver**:
   ```bash
   vserver nfs create -vserver nfs
   ```

## Step 2: Configure Export Policies

1. **Create Export Policies**:
   ```bash
   vserver export-policy create -vserver nfs -policyname nfs_vol
   vserver export-policy create -vserver nfs -policyname nfs_vol_q01
   ```
2. **Create Export Policy Rules**:
   ```bash
   vserver export-policy rule create -vserver nfs -policyname nfs_vol -ruleindex 1 -protocol nfs -clientmatch 172.16.96.12,172.16.128.12 -rorule any -rwrule never
   vserver export-policy rule create -vserver nfs -policyname nfs_vol_q01 -ruleindex 1 -protocol nfs -clientmatch 172.16.96.12,172.16.128.12 -rorule any -rwrule any
   ```

## Step 3: Create Network Interfaces

1. **Create LIFs for Data Access**:
   ```bash
   network interface create -vserver nfs -lif nfs_lif1 -role data -data-protocol nfs -home-node ontap7-01 -home-port e0e -address 172.16.0.6 -netmask-length 19
   network interface create -vserver nfs -lif nfs_lif2 -role data -data-protocol nfs -home-node ontap7-02 -home-port e0f -address 172.16.0.7 -netmask-length 19
   network interface create -vserver nfs -lif nfs_lif3 -role data -data-protocol nfs -home-node ontap7-01 -home-port e0e -address 172.16.0.8 -netmask-length 19
   network interface create -vserver nfs -lif nfs_lif4 -role data -data-protocol nfs -home-node ontap7-02 -home-port e0f -address 172.16.0.9 -netmask-length 19
   ```
2. **Create Default Route**:
   ```bash
   net route create -vserver nfs -destination 0.0.0.0/0 -gateway 172.16.0.1
   ```

## Step 4: Create and Configure Volumes

1. **Create Volumes**:
   ```bash
   volume create -vserver nfs -aggregate aggr1_ontap7_01_fcal -volume nfs_vol_a -size 50MB -junction-path /nfs_vol_a -security-style unix -policy nfs_vol -unix-permissions 755
   volume create -vserver nfs -aggregate aggr1_ontap7_01_fcal -volume nfs_vol_b -size 50MB -junction-path /nfs_vol_b -security-style unix -policy nfs_vol -unix-permissions 755
   ```
2. **Modify Root Volume Export Policy**:
   ```bash
   vol modify -vserver nfs -volume nfs_root -policy nfs_vol
   ```

## Step 5: Create and Configure Qtrees

1. **Create Qtrees**:
   ```bash
   qtree create -volume nfs_vol_a -qtree q01 -security-style unix -oplock-mode enable -unix-permissions ---rwxr-xr-x -vserver nfs -export-policy nfs_vol_q01
   qtree create -volume nfs_vol_b -qtree q01 -security-style unix -oplock-mode enable -unix-permissions ---rwxr-xr-x -vserver nfs -export-policy nfs_vol_q01
   ```

## Step 6: Create UNIX User for NFS Access

1. **Create UNIX User**:
   ```bash
   vserver services unix-user create -user ubuntua -id 1000 -primary-gid 1000
   ```

## Step 7: Verify Access to NFS Share

1. **Check Export Policy Access**:
   ```bash
   vserver export-policy check-access -vserver nfs -volume nfs_vol_a -client-ip 172.16.96.12 -authentication-method none -protocol nfs3 -access-type read-write -qtree q01
   ```

## Step 8: Linux Client Side - Mounting NFS Shares

1. **NSLookup for NFS Server**:
   ```bash
   nslookup 172.16.0.6
   ```
   Example Output:
   ```
   Server:  dns.server.address
   Address:  dns.server.address#53

   Name:    WinA.motors.com
   Address: 10.2.10.250
   ```

2. **Create Mount Directory**:
   ```bash
   sudo mkdir /mnt/nfs_dir_vol_a
   ```
3. **Mount NFS Share**:
   ```bash
   sudo mount WinA.motors.com:/nfs_vol_a /mnt/nfs_dir_vol_a
   ```

---

This guide is now polished and ready for distribution to your students. It provides a clear and comprehensive walkthrough of the NFS setup process on a NetApp ONTAP system.
