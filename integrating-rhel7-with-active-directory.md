Integrating RHEL7 with Active Directory

```
#vi /etc/selinux/config
```
Change enforcing to disabled

```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
#SELINUX=enforcing
SELINUX=disabled
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
    SELINUXTYPE=targeted
```
```   
#yum install -y chrony
#systemctl status chronyd.service
#chronyc sources -v
#cd /etc/
#vi /etc/chrony.conf
```
```
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
#server 0.centos.pool.ntp.org iburst
#server 1.centos.pool.ntp.org iburst
#server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst
# server ntp.ox.ac.uk iburst
    
# Ignore stratum in source selection.
stratumweight 0
    
# Record the rate at which the system clock gains/losses time.
driftfile /var/lib/chrony/drift
    
# Enable kernel RTC synchronization.
rtcsync
    
# In first three updates step the system clock instead of slew
# if the adjustment is larger than 10 seconds.
makestep 10 3
    
# Allow NTP client access from local network.
#allow 192.168/16
    
# Listen for commands only on localhost.
bindcmdaddress 127.0.0.1
bindcmdaddress ::1
    
# Serve time even if not synchronized to any NTP server.
#local stratum 10
    
keyfile /etc/chrony.keys
    
# Specify the key used as password for chronyc.
commandkey 1
    
# Generate command key if missing.
generatecommandkey
    
# Disable logging of client accesses.
noclientlog
# Send a message to syslog if a clock adjustment is larger than 0.5 seconds.
logchange 0.5
   
logdir /var/log/chrony
#log measurements statistics tracking

```

```
    # systemctl stop chronyd.service
    # systemctl status chronyd.service
    # systemctl start chronyd.service
    # systemctl status chronyd.service
    # chronyc sources -v
```

Edit /etc/nsswitch.conf

```
#vi /etc/nsswitch.conf
```

```
#passwd:     files sss winbind
#shadow:     files sss winbind
#group:      files sss winbind
##initgroups: files
    
passwd:     files compat
passwd_compat: winbind
shadow:     files winbind
group:      files winbind
```
Backup Samba Configuration file

```
#cd /etc/samba/
#cp smb.conf smb.conf-ORIG
```

Install config tools.

```
#sudo yum -y install authconfig-gtk sssd krb5-workstation system-config-authentication samba-winbind-clients.x86_64
#system-config-authentication
#wbinfo -u
```
This should produce a list of users example:

```
    ENG\engs1442
    ENG\admpetera
    ENG\npeu0282
    ENG\admdavids
    ENG\engs1440
    ENG\engs1439
    ENG\engs1438
    ENG\admscottr
    ENG\$vha100-c0o01i7otoh7
    ENG\admericp
```
```
    # wbinfo -g
    
    ENG\ibme_biomedia_interbio_rw
    ENG\access-inst-lab-usr
    ENG\access-inst-lab-adm
    ENG\ibme_qubic_glostrup_r
    ENG\ibme_qubic_glostrup_rw
    ENG\ibme_bsp_hd_ro
    ENG\ibme_bsp_ourr_ro
    ENG\ibme_bsp_ourr_rw
    ENG\ibme_bsp_ourr_results_rw
    ENG\ibme_bsp_hypo_study_rw
```

Edit smb.conf to include the following

```
    # vi smb.conf      
```
#Global Settings 
workgroup = ENG
obey pam restrictions = yes
realm = ENG.OX.AC.UK
security = ads
idmap config ENG:range = 10000-99999
idmap config ENG:backend = rid
idmap config * : range = 10000-99999
template homedir = /home/%D/%U
#template homedir = /home/%D/%G/%U
#template shell = /bin/bash
template shell = /sbin/nologin
winbind use default domain = true
winbind offline logon = true
winbind separator = +
winbind enum users = yes
winbind enum groups = yes
winbind expand groups = 8
winbind refresh tickets = Yes
wins server = 163.1.2.52
    
[global]
```
```
#systemctl restart winbind.service
#systemctl status winbind.service
```
    
Add the following lines at the bottom of /etc/passwd
```
#vi /etc/passwd
```
```   
   +engs0846:x::::/home/ENG/engs0846:/bin/bash
    +::::::/sbin/nologin
```    
Add the following lines at the bottom of /etc/group

```
vi /etc/group
```
```   
   +:::
```
