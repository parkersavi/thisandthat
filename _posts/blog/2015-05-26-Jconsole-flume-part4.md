---
layout: post
title: "Apache Flume Tutorial : How to monitor Flume using Jconsole"
modified:
categories: blog
excerpt:
tags: [flume,tutorial]
image:
  feature:
date: 2015-05-26T15:39:55-04:00
---
* Table of Contents
{:toc}

## Objective 
Configure Flume to monitor it using Jconsole 

--- 
 **Prerequesites** Jconsole. The jconsole executable can be found in JDK_HOME/bin

## Steps to Take on Server
 
**Step 1:** To monitor your running flume application using jconsole, go into your apache flume's conf directory
    {% highlight css %}
    cd apache-flume/conf
    {% endhighlight %}
 	
**Step 2:** Create <code>flume-env.sh</code> file if one doesn't exist under <code>apache-flume/conf</code> directory
**Step 3:** Inside <code>flume-env.sh</code> add the following
 	{% highlight css %}
	JAVA_OPTS="-Dcom.sun.management.jmxremote \
	-Dcom.sun.management.jmxremote.authenticate=false \
	-Dcom.sun.management.jmxremote.ssl=false \
	-Dcom.sun.management.jmxremote.port=[PORT NUMBER] \
	-Djava.rmi.server.hostname=[IP_ADDRESS_OF_YOUR_FLUME_SERVER]"
	{% endhighlight %}

 	**NOTE**: Make sure the PORT is open on your machine

**Step 4:** If flume is already running then restart it by killing the flume process

## Steps to Take on Local Machine
 
 
 **Step 1:**. After you have restarted flume, go on your local machine and type the following
     <code>jconsole 54.145.252.232:4234</code>  
     Note: format is <code>jconsole [IP]:[PORT]</code>
         where IP = your flume server's public IP
               PORT = port you chose in the above configuration

     
      
      
      
## Output
 This is what the output should look like if you got everything correctly
 ![](/images/this/flume/jconsole-1.png)
 
 ![](/images/this/flume/jconsole-2.png)

 
 
 
 
 
