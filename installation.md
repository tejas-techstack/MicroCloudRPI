## How To setup ubuntu server on raspberry pi:

1. Have raspberry pi imager installed
2. Insert SD card, and erase disk first.
3. Remove and reinsert sd card, choose rpi version, choose os and load onto the sd card.
4. insert into rpi and connect to power.
5. boom now os is installed and is up and running.

## How To establish ssh gateway to remotely access any ubuntu server:

### PreRequsites: 

Windows does not allow icmp natively, hence we must enable this,
so that a linux machine can ping a windows machine

[enable icmp in windows]('https://kb.iu.edu/d/aopy')  refer this for steps to enable firewall in windows

	1. Search for Windows Firewall, and click to open it.
	2. Click Advanced Settings on the left.
	3. From the left pane of the resulting window, click Inbound Rules.
	4. In the right pane, find the rules titled File and Printer Sharing (Echo Request - ICMPv4-In).
	5. Right-click each rule and choose Enable Rule.

1. Now on the ubuntu server, install openssh-server using
`sudo apt install openssh-server`

2. check if it is running using
`sudo systemctl status ssh`

3. then give permission to allow ssh through the ubuntu firewall(ufw)
`sudo ufw allow ssh`

SSH setup is ready


now run `ip a`(on ubuntu to get the ip address of the machine)

then run `ssh <name-of-ubuntu-server>@<ip-address>`
press yes on the prompt to establish the ssh handshake.
type the password of the server

and now you are connected to the ssh server remotely.
