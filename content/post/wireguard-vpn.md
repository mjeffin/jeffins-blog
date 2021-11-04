---
title: "Wireguard - Anybody can run a VPN now!"
date: 2021-11-4T07:44:59+05:30
draft: false
tags: ["networking","wireguard"]
---
# Introduction

We had developed a simple and easy to use Django app for a small mission hospital in remote town in the state of Bihar, India.
Usually we host our apps on AWS ec2, but unstable internet in the hospital and total cost of ownership turned out to be a challenge. 
We had to deploy it locally. So we decided to test it on a Raspberry Pi 3 Model B+ we had procured for another project.
We ran it continuously for 2 weeks, and it worked like a charm.

We needed a mechanism to access the pi remotely without the help of the hospital staff. There were a couple of options,

1. Use Raspbian GUI OS and use teamviewer
2. Setup DDNS and port forwarding in the router in hospital campus. 
3. Set up an always on VPN for management network

### TeamViewer
Running a GUI will result in more system load and we didn't want to do that just to setup teamviewer. With teamviewer, to run a commmand, you have to login using the GUI to open the terminal. And it will timeout after few minutes in the free version. You can't just have an ssh-tmux session running in the background. 

### DDNS
The hospital uses a Jiofy mobile hotspot for internet. It uses [Carrier-grade NAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) and you won't even get a public IP in your router to think about DDNS. I am not aware of any workarounds for it. 

### VPN

The next option is to use some sort of VPN. With VPN, both the hospital and the developer would connect to the same vpn gateway, and they can talk 
to each other via a private ip. One downside is that you would still need to have a vm in the cloud, but you can optimise it by switching it on
only when you need remote access. The various options are,
1. IPSec VPN - generally used for site to site vpn. usually requires dedicated firewalls
2. SSL VPN - generally used for corporate dial up vpn. Usually requires dedicated firewalls. [OpenVPN](https://openvpn.net/) is a flavour of SSL VPN
 used in personal or small business setting. But self-managing an openvpn instance is not very easy and the free version has restrictions
3. [Wireguard](https://www.wireguard.com/) - [New kid](https://en.wikipedia.org/wiki/WireGuard#History) on the block which is disrupting the personal vpn space.
[Endorsed](https://lists.openwall.net/netdev/2018/08/02/124) by Linus Torvalds, it's extremely fast to use, simple to manage and very secure. It's even added to 
the linux kernel now. The latest [Mozilla VPN](https://www.mozilla.org/en-US/products/vpn/) is using wireguard. 

After having tried to set up openvpn a few years back, we were amazed by how easy it was to set up wireguard and hence we decided to go ahead with it.
I would love to rave more about it, but due to lack of time, will leave you with the hn search [link](https://hn.algolia.com/?dateRange=all&page=0&prefix=true&query=wireguard&sort=byPopularity&type=story)
to read more. Let's so straight into the solution design and implementation for our problem at hand.


# Solution Design


![solution design](/wg.svg "solution design")

Although wireguard is peer to peer, we are using it in a hub and spoke topology. 

## IP Schema
Let's assume that the ip schema is as below for this example
1. Wireguard peer network - 10.49.0.0/24
2. Cloud/DC network - 10.10.0.0/16
3. LAN ip of the hub - 10.10.1.10
4. Supernet of all spoke sites - 10.20.0.0/16
5. Network behind spoke1 - 10.20.1.0/24

## Hub
Hub is nothing but any machine with a static public ip. We used an AWS ec2 instance with the lowest config for it. 
The best option to install wireguard on the hub is using [algo](https://github.com/trailofbits/algo), a command line based
ansible playbook to install and configure wireguard. Just follow the instructions and select _localhost_ as the target,
enter the right static public ip or hostname in the prompt and ensure to note the private keys to create the new users in the end. 
It is super simple and thanks to the [contributors](https://github.com/trailofbits/algo/graphs/contributors) to making life easier
for lots of people. Note that the goal of the project is to set up a _personal_ vpn server using wireguard. 

Now edit /etc/wireguard/wg0.conf file in the hub to edit the peers to include their subnets in allowed ip's. This will **automatically
inject** a route towards that subnet with the peer as the next hop! 

```
AllowedIps=10.49.0.2/32, 10.20.1.0/24
```

Algo script also installs and sets up iptables firewall and you will want to explicitly enable hub to spoke traffic

```
sudo iptables -I FORWARD 1 -s 10.10.0.0/16 -d 10.20.0.0/16 -j ACCEPT
sudo iptables -I FORWARD 1 -d 10.10.0.0/16 -s 10.20.0.0/16 -j ACCEPT
```

### Cloud/DC Router
If your have a proper supernet for your spokes, then you just need to define one single entry in the routing table for the servers to reach the spokes.
```
Network: 10.20.0.0/16; Next Hop: 10.10.1.10
```
Depending on your router/firewall model, you might want to disable ICMP redirects too, if your wg hub is in the same subnet as router.

## Spoke

Spoke is again nothing but any laptop/phone/raspberry pi connected to internet, but doesn't have a static public ip. Just [download](https://www.wireguard.com/install/)
and install for your platform. Then use the config file/QR code generated by the algo script. That's it. You will have a new virtual interface with ip in 
range of 10.49.0.0/24.

The modifications needed in the config file generated by algo are,

1. Change the _AllowedIps_ to have only routes you need. ie, 10.10.0.0/16 in our case. 0.0.0.0/0 is the default and will divert all your traffic to the wg hub. 
2. Remove _dns server_ entry or update it to point to your coroporate firewall

That's it. You are now protected by a vpn!

Now, if there are any devices behind the spoke device, you just need to set up routing in that device or router to say,

```
Network: 10.10.0.0/16; Next Hop: Lan ip of the spoke
```

Also don't forget to enable ip forwarding on your spoke device. If you running linux/raspi, enable wg to run on startup by

```shell
sudo systemctl enable wg-quick@wg0.service
```

# Cost
Wireguard doesn't need much computing resources. 1 vpc with 1 gb ram should be good enough for few users. If you using it for ad-blocking and passing all internet
traffic, you might need to watch out for cloud egress costs, especially while streaming videos.

For our scenario of remote access, we can just keep the cloud vm shut down and start it only when we need to access the remote server. We just need to pay by the hour that way. 

# Spoke to spoke traffic
Spokes can talk to each other via 10.49.0.0/24 addresses. It is disabled by default using iptables if you installed via algo, but can be enabled by editing the rules.
That is what we used to talk to the raspi in the remote hospital.

# Cost
In the scnario of remote manae

# Conclusion

Wireguard VPN is something quite trivial to set up on your 5$ a month vm and static public ip, in a country of your choice. In this age of
draconian surveillance, that is worth it. 

I'm working on an open source project to integrate wireguard with OIDC and 2FA providers like okta for use by small enterprises and will share the details soon.