## Adding an Ubuntu Machine to Windows Active Directory Using Kerberos

### 1. **Install Necessary Packages on Ubuntu**

Ensure your Ubuntu machine has the required packages installed. Open a terminal and run:

```bash
sudo apt update
sudo apt install realmd sssd adcli samba-common-bin krb5-user
```
![Pasted image 20240806084510](https://github.com/user-attachments/assets/b7397c7b-3e64-43b8-9c10-68ca39635895)

### 2. **Configure Kerberos**

During the installation, you’ll be prompted to configure Kerberos. You can also manually edit the Kerberos configuration file:

```bash
sudo nano /etc/krb5.conf
```

Ensure it includes the necessary settings for your AD domain. Example configuration:

```bash
[libdefaults]
    default_realm = WINB.COM
    dns_lookup_realm = true
    dns_lookup_kdc = true

[realms]
    WINB.COM = {
        kdc = winb.winb.com
        admin_server = winb.winb.com
    }
    ATHENA.MIT.EDU = {
        kdc = kerberos.mit.edu
        admin_server = kerberos.mit.edu
    }
    ANDREW.CMU.EDU = {
        admin_server = kerberos.andrew.cmu.edu
        default_domain = andrew.cmu.edu
    }
    CS.CMU.EDU = {
        kdc = kerberos-1.srv.cs.cmu.edu
        kdc = kerberos-2.srv.cs.cmu.edu
        kdc = kerberos-3.srv.cs.cmu.edu
        admin_server = kerberos.cs.cmu.edu
    }
    DEMENTIA.ORG = {
        kdc = kerberos.dementix.org
        kdc = kerberos2.dementix.org
        admin_server = kerberos.dementix.org
    }
    stanford.edu = {
        kdc = krb5auth1.stanford.edu
        kdc = krb5auth2.stanford.edu
        kdc = krb5auth3.stanford.edu
        master_kdc = krb5auth1.stanford.edu
        admin_server = krb5-admin.stanford.edu
        default_domain = stanford.edu
    }
    UTORONTO.CA = {
        kdc = kerberos1.utoronto.ca
        kdc = kerberos2.utoronto.ca
        kdc = kerberos3.utoronto.ca
        admin_server = kerberos1.utoronto.ca
        default_domain = utoronto.ca
    }

[domain_realm]
    .winb.com = WINB.COM
    winb.com = WINB.COM
    .mit.edu = ATHENA.MIT.EDU
    mit.edu = ATHENA.MIT.EDU
    .media.mit.edu = MEDIA-LAB.MIT.EDU
    media.mit.edu = MEDIA-LAB.MIT.EDU
    .csail.mit.edu = CSAIL.MIT.EDU
    csail.mit.edu = CSAIL.MIT.EDU
    .whoi.edu = ATHENA.MIT.EDU
    whoi.edu = ATHENA.MIT.EDU
    .stanford.edu = stanford.edu
    .slac.stanford.edu = SLAC.STANFORD.EDU
    .toronto.edu = UTORONTO.CA
    .utoronto.ca = UTORONTO.CA
```

Replace `WINB.COM` and the related details with your actual domain name and update KDC and admin server details as required.

### 3. **Configure SSSD**

Ensure that the SSSD configuration is correctly set up:

```bash
sudo nano /etc/sssd/sssd.conf
```

Sample configuration:

```ini
[sssd]
services = nss, pam
config_file_version = 2
domains = winb.com

[domain/winb.com]
id_provider = ad
access_provider = ad
override_homedir = /home/%u@%d
default_shell = /bin/bash

krb5_realm = WINB.COM
realmd_tags = manages-system joined-with-samba

cache_credentials = True
ad_domain = winb.com
ldap_id_mapping = True
use_fully_qualified_names = False
fallback_homedir = /home/%u@%d
access_provider = simple
```

### 4. **Join the Ubuntu Machine to the AD Domain**

Use the `realm` command to join the domain. You will need domain admin credentials:

```bash
sudo realm join --user=adminuser@winb.com winb.com
```

You will be prompted to enter the password for the domain admin account.

### 5. **Restart SSSD**

Restart the SSSD service to apply changes:

```bash
sudo systemctl restart sssd
```

### 6. **Configure PAM to Create Home Directories Automatically**

Edit the PAM configuration for SSSD:

```bash
sudo nano /etc/pam.d/common-session
```

Add the following line at the end of the file to ensure home directories are created for domain users:

```ini
session required pam_mkhomedir.so skel=/etc/skel/ umask=0022
```

### 7. **Update Windows Kerberos Registry Settings**

On your Windows machine, update the Kerberos registry settings to ensure compatibility and proper functioning. Open the Registry Editor (`regedit`) and navigate to:

```plaintext
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa\Kerberos\Parameters
```

![[Pasted image 20240806091739.png]]
![[Pasted image 20240806091802.png]]
![[Pasted image 20240806091837.png]]
![[Pasted image 20240806091910.png]]
![[Pasted image 20240806092015.png]]
![[Pasted image 20240806092114.png]]
If the `Parameters` key does not exist, you need to create it. Add or update the following DWORD values as needed:

- **AllowTgtSessionKey**: Set this to `1` to allow TGT session key.
- **LogLevel**: Set this to `0` for basic logging or `1` for detailed logging.

To add a new value:

1. Right-click on the `Parameters` key.
2. Select `New` -> `DWORD (32-bit) Value`.
3. Enter the name of the value and press Enter.
4. Double-click the value and set the desired data.

After making these changes, reboot your Windows machine for the changes to take effect.

### 8. **Verify the Configuration**

To verify the setup, attempt to log in with a domain user:

```bash
su - ad_user
```

Replace `ad_user` with a valid AD username. If everything is set up correctly, you should be able to log in.

### 9. **Test Domain User Login**

Try logging in with a domain user again to ensure that the home directory is created:

```bash
su - u1@winb.com
```


[Proxmox Lab Setup and Configuration Guide](Proxmox%20Lab%20Setup%20and%20Configuration%20Guide.md)
