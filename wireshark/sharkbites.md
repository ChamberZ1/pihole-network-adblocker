# Description
This file serves as documentation of my use of Wireshark to analyze and understand my DNS traffic and Pi-hole in action.

### Web Proxy Auto-Discovery (WPAD)
Upon opening my Wireshark and capturing from Wi-Fi, I was faced with a bunch of existing queries, leaving me clueless on where to start. So, I restarted the capturing and noticed that I had a repeating set of DNS queries for `wpad.lan`. 

![wpad.lan queries](wpad-lan.png)

So what is `wpad.lan`, and why is it being queried so often? Surely it's something important, perhaps related to security or updates? Nope, WPAD stands for [Web Proxy Auto-Discovery](https://en.wikipedia.org/wiki/Web_Proxy_Auto-Discovery_Protocol), a protocol "used to ensure all systems in an organization use the same web proxy configuration. Instead of individually modifying configurations on each device connected to a network, WPAD locates a proxy configuration file and applies the configuration automatically" (Source: [CISA.gov](https://www.cisa.gov/news-events/alerts/2016/05/23/wpad-name-collision-vulnerability)). Supposedly, WPAD first tries to obtain the file via DHCP, then DNS (which is how we discovered this on Wireshark), and finally Link-Local Multicast Name Resolution (LLMNR) and/or NetBIOS. So I filtered for DHCP packets to verify this and found that in fact, within the `Option (55) Parameter Request List` included in the DHCP packets, is list item `(252) Private/Proxy autodiscovery` which is saying to the DHCP server: "If you support option 252 (WPAD URL), please send it to me". 
![DHCP autodiscovery](252autodiscover.png)

As for LLMNR/NetBIOS, I asked ChatGPT how to test if I have LLMNR enabled, and it told me to ping a nonexistent hostname while capturing on Wireshark with an `llmnr` filter. If I see LLMNR packets, then I have it enabled. Evidently, I have LLMNR enabled.

![LLMNR enabled](llmnr-enabled.png)

Having WPAD, LLMNR, and NetBIOS on is not safe if you use your machine in a public network.
LEFT OFF NOTE: 
1) FIND OUT IF IT'S IMPORTANT TO DISABLE LLMNR/NetBIOS + anything else on private home networks. as well as public networks like in airport and cafe.
2) If unsafe, find out how to turn off.
3) Need to do more research on why these are bad... then write brief explanation and MOVE ON. THIS IS NOT MY FOCUS ATM. 

