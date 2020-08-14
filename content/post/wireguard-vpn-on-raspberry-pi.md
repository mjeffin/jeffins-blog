---
title: "Wireguard VPN on Raspberry Pi"
date: 2020-08-14T07:44:59+05:30
draft: true
---
Recently we developed a simple and easy to use Django app for a small mission hospital in remote town in the state of Bihar, India. Usually we host out apps on AWS ec2, but unstable internet in the hospital and total cost of ownership turned out to be a challenge. We had to deploy it locally. So we decided to test it on a Raspberry Pi 3 Model B+ we had procured for another project. We ran continously for 2 weeks and it worked like a charm. More on that in another post. 

We needed a mechanism to access the pi remotely without the help of the hospital staff. There were couple of options,

1. Use Raspbian GUI OS and use teamviewer
2. Setup DDNS and port forwarding in the router in hospital campus. 
3. Set up an always on VPN for management network

### GUI
Running a GUI will result in more system load and we didn't want to do that just to setup teamviewer. With teamviewer, to run a commmand, you have to login using the GUI to open the terminal. And it will timeout after few minutes in the free version. You can't just have an ssh-tmux session running in the background. 

### DDNS
The hospital uses a Jiofy mobile hotspot for internet. It uses [Carrier-grade NAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) and you won't even get a public IP in your router to think about DDNS. I am not aware of any work arounds for it. 

### VPN

WIP..
