---
layout: post
title: Ubuntu Automation with Multipass
date: 2020-07-18 00:00:00 +1000
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: multipass-logo-jpg.jpg # Add image post (optional)
tags: [Productivity, Software] # add tag
---

Time to time one may need to spin up Linux virtual machines for testing etc and it can be really time consuming starting over every time downloading the images and installing them. Multipass solves that problem by allowing you to create disposable Ubuntu VMs instantly with few strokes of your keyboard.
Here is a simple step by step guide to launch Ubunut VMs using Multipass.

1-First you need to download Multipass from https://multipass.run.

2-Choose your host operating system . In my case, I will be using Windows 10 with Virtual Box.

3- Download and run the executable and follow the installation prompts

![]({{site.baseurl}}/assets/img/install-multipass.png)

![]({{site.baseurl}}/assets/img/install-multipass2.png)

![]({{site.baseurl}}/assets/img/install-multipass3.png)

![]({{site.baseurl}}/assets/img/install-multipass4.png)

![]({{site.baseurl}}/assets/img/install-multipass5.png)

4-Bring up Windows command prompt and execute the following command:
{% highlight shell %}
C:\Users\User>multipass launch --name <name of your VM>
{% endhighlight %}

the parameter --name is to give a name to your instance in my case I am naming it clustermaster as this will be its role for my Splunk Cluster.

![]({{site.baseurl}}/assets/img/command-1.png)

Multipass starts to launch the image in the background and once the process is completed you will see the following message.
![]({{site.baseurl}}/assets/img/command-2.png)

5-Check and verify if the instance is running:

{% highlight shell %}
C:\Users\User>multipass list
{% endhighlight %}

![]({{site.baseurl}}/assets/img/command-3.png)

We can see my newly launched instance is in running state and has Ubuntu 18.0.04.

6- To interact with our VM we have couple of options. We can execute the commands directly from multipass prompt by using exec command

{% highlight shell %}
C:\Users\User>multipass exec <vm name> --<command>
{% endhighlight %}
![]({{site.baseurl}}/assets/img/command-4.png)

Here I executed ‘uname -a’ command from multipass prompt directly in my vm named “clustermaster”. This is a quick and dirty way to interact with your vm but if you would like to have a complete interactive session you can create a shell between host and the VM by executing the following command:
{% highlight shell %}
C:\Users\User>multipass shell <name of your VM>
{% endhighlight %}
![]({{site.baseurl}}/assets/img/command-5.png)

7-(Optional) Once the launch is complete download PSTools from Windows Sysinternal site and extract the zip file to your Download folder.Then bring up PowerShell as admin and execute the following command:
& $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s -i $env:VBOX_MSI_INSTALL_PATH\VirtualBox.exe
This allows you to view and manage the instances in Virtualbox otherwise you will not be able to see them in VBox like other VMs that you install manually.

![]({{site.baseurl}}/assets/img/vmware-1.png)

And that’s it! You ‘ve got you instance of Ubuntu up and running in no time. Once you are done with your testing you can simply shut it down by executing:
{% highlight shell %}
C:\Users\User>multipass shutdown vmname
{% endhighlight %}

or can delete and purge the VMs no longer needed by executing the following commands.:
{% highlight shell %}
C:\Users\User>multipass delete vmname
{% endhighlight %}
{% highlight shell %}
C:\Users\User>multipass purge
{% endhighlight %}
To sum up,Multipass is a handy tool to quickly initiate Ubuntu instances and create your own “mini cloud” as the folks at Canonincal,the author of the tool put it. The tool supports metadata for cloud-init, so it’s possible to simulate a mini version of a cloud deployment from your on-prem devices even your laptop.
