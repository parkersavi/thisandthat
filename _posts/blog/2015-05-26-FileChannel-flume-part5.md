---
layout: post
title: "Apache Flume Tutorial : Use File Channel in stead of memory channel"
modified:
categories: blog
excerpt:
tags: [flume,tutorial]
image:
  feature:
date: 2015-05-27T15:39:55-04:00
---
* Table of Contents
{:toc}

## Objective 
To tail a continous filling log file and use Flume's file channel as the buffer in stead of memory channel and simply log the output on console

--- 
 
"MemoryChannel provides high throughput but loses data in the event of a crash or loss of power. As such the development of a persistent Channel was desired. FileChannel was implemented in FLUME-1085. The goal of FileChannel is to provide a reliable high throughput channel. FileChannel guarantees that when a transaction is committed, no data will be lost due to a subsequent crash or loss of power." [more info](https://blogs.apache.org/flume/entry/apache_flume_filechannel)
 

**What we will do**: We will tail a streaming log file, use file channel as buffer to store data on disk and simply show the output to console


![](/images/this/flume/file_channel.png)



## Setup configuration on Server
--- 



 
**Step 1:** Flume will require two directories for File Channel to work

   &nbsp;&nbsp;&nbsp;&nbsp;i) Data Directory (to store data)   
   &nbsp;&nbsp;&nbsp;&nbsp;ii) Checkpoint directory - Flume also uses in memory queue with the file channel. Basically checkpoint is the state of queue that is written to the disk in the checkpoint directory.
      {% highlight css %}
           mkdir apache-flume/data 
mkdir apache-flume/checkpoint
      {% endhighlight %}
     
     
     
     
**Step 2:** Go to the conf directory of your flume installation.
           
 	 {% highlight css %}
 	 cd apache-flume/conf/
 	 {% endhighlight %}
 	 
 	 
 	 
 	 
**Step 3:** Write configuration file <code>flume_file_channel.conf</code> and put it in your conf directory
          
{% highlight css %}  
# configure the agent
   agent.sources = r1
   agent.channels = k1
   agent.sinks = c1

# use FILE channel
    agent.channels.k1.type = file
    agent.channels.k1.transactionCapacity = 100
    agent.channels.k1.checkpointDir = /home/apache-flume/checkpoint  
    			#NOTE: point to your checkpoint directory
    agent.channels.k1.dataDirs = /home/apache-flume/data   
    			#NOTE: point to your data directory

# connect source, channel, sink 
    agent.sources.r1.channels = k1
    agent.sinks.c1.channel = k1

# tail the file. -F flag is used to continously tail the file even if the file is rolled
    agent.sources.r1.type = exec
    agent.sources.r1.command = tail -F /home/ec2-user/some.log  
    					#NOTE: change the above path

# print the data to the screen using logger
    agent.sinks.c1.type = logger
 {% endhighlight %}

Save the above configuration in a file. Let's call it <code>flume_file_channel.conf</code> and put it under conf directory



   
   
## Run the example  
--- 

**ON SERVER**
     {% highlight css %}
cd apache-flume/

bin/flume-ng agent --conf conf --conf-file conf/flume_file_channel.conf --name agent -Dflume.root.logger=INFO,console
 	 
 	 {% endhighlight %}
     
      
      
      
## Output
---

 When flume runs, it should WRITE the contents of your tailed file on to the console. 
 
 ![](/images/this/flume/file_channel_output.png)

 
 
 
 
 
