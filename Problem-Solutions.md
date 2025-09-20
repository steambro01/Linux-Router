## Some of the problems I ran into and the solutions I found to solve those problems

- Needing to add more interfaces through the Virtual Box gui

- Misconfiguring the interface name (enpos9 instead of enp0s9) in Netplan config file

- Not having a true understanding of different interface types inside of Virtual Box (ex. NAT, Bridged, Host only network, etc.) and their      use cases

- Not being able to ssh into the Management interface I created. I verified ssh was enabled, I could ping the interface IP address, and I had the correct username. After going back and forth with chatgpt about my symptoms and having no luck, I realized that my macbook had new bridged interfaces labled "bridge100-102" with the same IP address as the three interfaces I just created on a Host only network. This is what was stopping the ssh so I changed my managment interface adapter from "Host only network" to "NAT" and enabled port forwording, now I ssh onto the Managment port using "ssh -p 2222 <steambro>@127.0.0.1". Once I removed the static IP address from the Netplan file I was able to successfuly ssh into the router this way

- Couldn't ping Kali VM after initial configuration. After enabling promiscuous mode to "Allow all" I was able to ping it. Promiscuous mode allows the virtual NIC to accept all network traffic and not just the traffic assigned to its own MAC address

- Not hitting "commit change" after deleting github file from repo and being confused why it wouldn't actually delete (it's been a while
