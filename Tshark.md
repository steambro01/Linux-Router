# Tshark

## This file will track my progress using wireshark/tshark to monitor network traffic to and from my router simulating a NOC environment

### Screenshot of Tshark being installed and capturing packets on one of my router interfaces. The image shows tshark's log of a device on the LAN being captured running an nmap scan to view all active IP addresses on its network
<img width="1546" height="1452" alt="Tshark" src="https://github.com/user-attachments/assets/bd678e91-c612-49b1-9e38-32b58311562e" />

``` bash
# update local package list and install tshark
sudo apt update
sudo apt install tshark

# shows availble interfaces to capture packets on
sudo tshark -D

# capture packets on enp0s9 interface
sudo tshark -i enp0s9
```
