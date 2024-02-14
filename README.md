# Monitoring IPsec tunnels on PFSense using zabbix

This project was forked from https://github.com/alanwds/zabbix_ipsec_pfsense. Thanks to @alanwds and @smejdil for their work. 

These scripts are used for monitoring IPSEC tunnels on PFSense v22.01 using zabbix.

# Dependencies

- Zabbix agent 5.4 (you can install it from pfsense packages manager)
- sudo (you can install it from pfsense packages manager)
- Zabbix Server >= 5.4
- check_ipsec.sh
- check_ipsec_traffic.sh
- zabbix-ipsec.py
- zabbix_sudoers

# How it works

The script zabbix-ipsec.py for tunnels ids (conX). After that, the zabbix items are created with the check_ipsec.sh script. The script check_ipsec_traffic.sh is used to collect traffic metrics about a given tunnel.

### Installation

- You have to upload check_ipsec.sh, check_ipsec_traffic.sh and zabbix-ipsec.py on pfsense filesystem. (/usr/local/bin/ in this example)
- Install sudo pakage at pfsense packages manager
- Copy file zabbix_sudoers under /usr/local/etc/sudoers.d
- Enable Custom Configuration on Advanced Settings at System -> sudo
- Create the following user parameters on zabbix-agent config page on pfsense (Service -> Zabbix-agent -> Advanced Options)
```
UserParameter=ipsec.discover,/usr/local/bin/python3.11 /usr/local/bin/zabbix-ipsec.py
UserParameter=ipsec.tunnel[*],/usr/local/bin/sudo /usr/local/bin/check_ipsec.sh $1
UserParameter=ipsec.traffic[*],/usr/local/bin/sudo /usr/local/bin/check_ipsec_traffic.sh $1 $2
```
- Set execution permissions
```
chmod +x /usr/local/bin/zabbix-ipsec.py
chmod +x /usr/local/bin/check_ipsec.sh 
chmod +x /usr/local/bin/check_ipsec_traffic.sh 
``` 
- Import the template ipsec_template.xml on zabbix and attach to pfsense hosts
- Grab a cup of coffee
