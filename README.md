# RedProxyControl
Tool for proxying all http and https traffic From a VM guest to host. Meant for use with Burp Suite.

I like to use a combination of Windows and Linux operating systems for all my hacking. I have Burp Suit installed on Windows but I also like some of the tools and ease of use with Linux. The idea here is for any tool I use in my Linux VM that makes an http / https request, for it to get sent to Burp so I can see the Request and response in Burp which for me is located on my Windows host. This means I have to use a proxy to traffic request to my proxy... anyway to accomplish this I created a short script you can run which easily turns on and off redsocks proxy and iptalbe rules in you Linux VM.

## Installation  

- First download redsocks on your VM, wheather thats Kali / Ubuntu / Mint or whatever your flavor of Linux distro is.

`sudo apt update`
`sudo apt install redsocks`

 - Now that redsocks is installed navigate to redsocks.conf, this should be located in the /etc/redsocks.conf. User your favorite text editor to open the file.

`cd /etc/redsocks.conf`
`subl /etc/redsocks.conf` instead of subl nano or vim will work or really anything

- One the file is opened you will have to change some settings. Under redsocks function you have to change the IP address to your Windows host and the port your Burp Proxy is listening on, mine is 8080. The last thing you will want to change is the known types to http-connect. Here is what my redsocks.conf file looks like.

```
redsocks {
	/* `local_ip' defaults to 127.0.0.1 for security reasons,
	 * use 0.0.0.0 if you want to listen on every interface.
	 * `local_*' are used as port to redirect to.
	 */
	local_ip = 127.0.0.1;
	local_port = 12345;

	// `ip' and `port' are IP and tcp-port of proxy-server
	// You can also use hostname instead of IP, only one (random)
	// address of multihomed host will be used.
	ip = 192.168.1.29;
	port = 8080;


	// known types: socks4, socks5, http-connect, http-relay
	type = http-connect;

	// login = "foobar";
	// password = "baz";
```

- The redsocks proxy is not set up.
- Head over to Burp Suite and in Proxy settings click exit under Proxy Listeners and select "All Interfaces" for "Bind to address".
- NOTE: you may have to go into your windows firewall and allow traffic below is a picture of the location I applied the settings to allow traffic through.
![image](https://github.com/AlbertL7/RedProxyControl/assets/71300144/fca8f3aa-a699-4e17-853c-07c61585608d)

- Take the script and add it to PATH, I personally added it to `/usr/local/bin/redproxycontrol.sh`
- After you have addeed it to path make it executable `sudo chmod +x /usr/local/bin/redproxycontrol.sh`
- To make things easy I added an alias to my `.zshrc` profile to easily turn the proxy on and off.

`alias proxyon='sudo /usr/local/bin/redproxycontrol.sh start'`
`alias proxyoff='sudo /usr/local/bin/redproxycontrol.sh stop'` 

- After this you should be ready to go, this will proxy traffic from your Linux VM to Burp Suite located on you Windows host.
<img src="https://giphy.com/embed/83HW1W3z0ttihuRAgS" width="300" height="200">
![image](https://github.com/AlbertL7/RedProxyControl/assets/71300144/0a2503e1-9100-44df-81ed-40774cd44b89)



