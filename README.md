# Linux-Router
Ubuntu Server configured as a virtual router to provide routing, NAT, and VLAN grouping for virtual machines and home lab devices. Includes configurations for Netplan and inter VLAN routing to simulate enterprise style networking on a personal setup.

## ❓ Why use a Linux server as a router?

💡 Some benefits include;
```

	- Being able to use low cost hardware or hardware you already own
 
	- Flexibility of being in control of how the router behaves such as NAT, firewall rules, DHCP, VLANS, etc
 
	- Lightweight and efficent enough for homelabs and small to medium size establishments
 
	- Can use one device as a router, DNS/DHCP server, web server, and much more
 
	- Can be easily replicated and scaled
 
	- Can be used to bridge cloud environments
 ```
	
## Software needed
	- Virtual Box (or any virtualizatoin platform you prefer)
	- Ubuntu Server iso
 	- Other VM's to connect to your LAN
	- ChatGPT ( for troubleshooting)
		
## Hardware needed
	- Computer that supports virtualization
	- Home router to provide internet access via NAT (or just modem but will require a few more steps)

## Instillation and configuration

Once you have your hyporvisor installed and running with ubnutu server downloaded, you want to first add at least two virtual interfaces/NIC's

** NAT for WAN facing interface and one Host-only Network for the each VLAN **

Next you want to enable port fowarding on your NAT interface. Under the NAT adapter click port forwarding > add > {host ip 127.0.0.1 , host port 2222 , guest port 22}

If the adapters didn't automatically generate a MAC address you can hit the wheel next to the text box to generate one for each adapter. * Each adapter must have a MAC address to continue *

Then you want to add VLANS/subnets to your Host-only network pool. In virtual box click tools > Network > Create > click the network and give it a name > assign the network a subnet mask and IP range


## Your network settings should look like this in Virtualbox

<img width="938" height="174" alt="Screenshot 2025-09-20 at 12 16 00 PM" src="https://github.com/user-attachments/assets/232ca841-9bc2-43ce-9a18-75da0444af14" />

😎 Now your ready to start the server. This is where the fun starts !

### Start the server

After setting up the server for the first time and signing in, back out to the root directory (username:/$)

``` bash
# back out to the root directory (username:/$)
cd ..
cd ..

# Get each interface name
ip addr 
```
Going off the provided image, the first is the loopback interface , then the NAT interface which was automatically configured by virtual box, then however many adapters you added which still need configuring
<img width="1980" height="680" alt="Screenshot 2025-09-20 at 12 38 01 PM" src="https://github.com/user-attachments/assets/72b32971-0b7a-49de-b3df-f090ee4f9070" />

``` bash
# enter netplan folder
cd etc/netplan

# view your netplan config file name
ls

# edit the config file
sudo nano 'filename'
```
It should look similar to the image below. So far, only the NAT interface has been added. We will add the other interfaces using the interface name we just viewed and the IP address we created when we created our subnets

<img width="3094" height="1974" alt="Screenshot 2025-09-20 at 12 34 30 PM" src="https://github.com/user-attachments/assets/59cec096-ed5c-4e8c-937e-6c40a291a568" />

After adding your configurations, your netplan file should look something like the image below. See netplan documentation for proper syntax and format

<img width="1194" height="926" alt="new netplan" src="https://github.com/user-attachments/assets/d2abb865-6de6-421b-bd6c-f699f340ea70" />
``` bash
ctr + x
y
return/enter
sudo netplan apply
``` 
If you get an error there is something wrong with the syntax. Make sure every thing is aligned, you used spacebar instead of tab, and you used the  right arrow for new line instead of enter
 ``` bash
ip addr 
```
Now you should now see an ipv4 address and subnet mask next to each interface like the image below
<img width="1980" height="1022" alt="new ip addr" src="https://github.com/user-attachments/assets/5d155258-aa49-4a11-9fba-de4569b1a96c" />

## Enabling SSH
``` bash
cd ..
cd ..
sudo systemctl status ssh
```
If 'ssh.service' and 'preset' aren't enabled and is doesn't say 'active (running)' then run the following commands

``` bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```
Now it should look something like this 

<img width="959" height="322" alt="Screenshot 2025-09-20 at 6 05 15 PM" src="https://github.com/user-attachments/assets/6d6a5fc7-8ac0-4d00-a0bc-e5a0ff00f039" />

Now from your host device
``` bash
ssh -p 2222 'username'@127.0.0.1
```
After entnering password you have now used ssh to remote control your router from the NAT port

## Adding hosts to your LAN

Now you can create as many vm's as your subnet allows and assign them to ip address range of your VLANS

## Links
```
# Netplan documentation
https://netplan.readthedocs.io/en/stable/

# Ubuntu Server documentation
https://documentation.ubuntu.com/server/how-to/#how-to
