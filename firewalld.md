# Firewalld

Firewalld is a host based firewall management tool for linux. Another popular option I was debating between was UFW. I chose firewalld because it is most commonly used in enterprise environments due to it being dynamic and zone based. Based on research, firewalld is just the user friendly front end to Netfilter.

## Installing firewalld

``` bash
sudo apt update
sudo apt install firewalld -y
```
<img width="1438" height="1692" alt="Installing firewalld " src="https://github.com/user-attachments/assets/acffe8cb-7014-43d3-bb99-9aa361489543" />

## Starting firewalld

``` bash
# enable makes it so it starts back up after server shutdown
sudo systemctl enable firewalld

# start actually starts the service
sudo systemctl start firewalld

# Verify it is running
sudo systemctl status firewalld
```
<img width="1436" height="522" alt="start:status firewalld" src="https://github.com/user-attachments/assets/e4172850-e497-4e87-a879-45ba716dc376" />

## Getting familiar

``` bash
# default zone is the zone where all traffic is automatically assigned
firewall-cmd --get-default-zone

# Shows all created zones
firewall-cmd --get-zones

# Shows the complete configuration for the default zone
firewall-cmd --list-all
```
### Creating and configuring zones for LAN1 and LAN2
``` bash
firewall-cmd --permanent --new-zone=lan1
firewall-cmd --permanent --zone=lan1 --add-source=192.168.50.0/29
firewall-cmd --permanent --zone=lan1 --add-service=ssh
firewall-cmd --permanent --new-zone=lan2
firewall-cmd --permanent --zone=lan2 --add-source=192.168.50.8/29
firewall-cmd --reload
```
## Config cheatsheet
```bash 
# Create new zone
firewall-cmd --permanent --new-zone=lan1
# Reload to apply
firewall-cmd --reload
# get default zone
firewall-cmd --get-default-zone
# show all predefined zones
firewall-cmd --get-zones
# view config for zone
firewall-cmd --zone=lan1 --list-all
# allow services
firewall-cmd --permanent --zone=lan1 --add-service=ssh
firewall-cmd --permanent --zone=lan1 --add-service=https
# open custom port
firewall-cmd --permanent --zone=lan1 --add-port=8080/tcp
# assign interface to zone
firewall-cmd --permanent --zone=lan1 --add-interface=enp0s9
# assign ip address or entire network range to zone
firewall-cmd --permanent --zone=lan1 --add-source=192.168.50.0/29

```
