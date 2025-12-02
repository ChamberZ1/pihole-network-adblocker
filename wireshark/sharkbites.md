# Description
This file serves as documentation of my use of Wireshark to analyze and understand my DNS traffic and Pi-hole in action.

### Web Proxy Auto-Discovery (WPAD)
Upon opening my Wireshark and capturing from Wi-Fi, I was faced with a bunch of existing queries, leaving me clueless on where to start. So, I restarted the capturing and noticed that I had a repeating set of DNS queries for `wpad.lan`. 

![wpad.lan queries](wpad-lan.png)
