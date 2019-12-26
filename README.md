## Folderwatch

##### A simple application that listens to a folder and executes an event script on the creation of a file

- [Installation](#requirements)
- [Startup](#startup)
- [Verify Running](#verify-running)
- [Event Script](#the-event-script)
- [Killing Processes](#stopping-processes)
- [Expandability](#expandability)

  
<br />

#### Requirements
1. Linux system with bash
2. inotify-tools  
```apt-get -y install inotify-tools```
<br />  

#### Installation
1. ```cd /etc/```
2. ```git clone https://github.com/mrspeiser/folderwatch.git```

I recommend cloning the repository into /etc/ because /etc/ is generally for etcetera application and will be easy to access for starting a listener from any working directory.  
<br />  

#### Startup

1. Navigate to the directory you want to listen in

  ```cd /home/drop/myspecialfolder```

2. Run this command which creates a hidden directory, named pipe, and then copies neccessary specific files from /etc/folderwatch/ into hidden directory. Then it starts an inotify daemon that sends stdout to the named pipe.
  
```/etc/folderwatch/inotify-event-listener```
   - You can also pass the name of the pipe as the first argument, otherwise it will prompt you 
```/etc/folderwatch/inotify-event-listener "mypipename"```

3. Enter the hidden directory 
  
```cd /home/drop/myspecialfolder/.folderwatch```

4. Start the listener to forward the output from the pipe to the event
  
```./startListener```
  
   - You can also pass the pipename as the first parameter  
```./startListener "mypipename"```
  
   - Additionally you can also manually start the listening process by executing the listener as a background process. The startListener script is just a safer prompted default shortcut. 
```./listener "mypipename" &```  
<br />

#### Verify Running

There are 2 simple useful scripts that get copied into the hidden folder to view and filter processes:
1. ps_inotify
2. ps_listener

The **ps_inotify** will show you all the inotify processes currently running. If you do not see the folder that you started the process in, you will need to delete the hidden folder and start over.

The **ps_listener** will show you all the named-pipe processes currently running. If you do not see the pipe running you will need to start the listener again by running the **startListner** script inside the hidden folder.  
<br />

#### The Event Script

The event script fires everytime a file is created in the directory you chose and the inotify-process that's listenening in that directory outputs to the pipe. The pipe feeds that output into the event.

The default behavior for the event script is to log the output to the log.txt file inside the .folderwatch folder.

The output to the log will look something like this:  

inotify output: CREATE,THIS-FILE-WAS-JUST-CREATED.txt    --    timestamp: Fri Apr  5 19:15:22 UTC 2019

You can see the above output as the default action inside the event script.

You _DO NOT_ need to stop the listener or any processes to modify the event script. The event script runs independently and only gets triggered when it receives input from the pipe.  
<br />  

#### Stopping Processes

There is no script to automatically kill an inotify daemon or a folderwatch listener. The process numbers are volatile and change based on system resource needs.

To kill a inotify daemon use the **ps_inotify** script to first identify the process id which is the first column and type ```kill 12483``` where 12483 is the process id.

To kill a folderwatch listener use the **ps_listener** to first identify the process id for the listener you want to kill. Then type ```kill 12488``` where 12488 is the process id.  
<br />  

#### Expandability

The inotify daemon that is started can be found in the inotify-event-listner script and it looks like:

```inotifywait -d -e create "$(pwd)" -o "$(pwd)/.folderwatch/$pipename" --timefmt "%a, %d %b %Y %T %z" --format "%e,%f"```


The inotifywait command takes a parameter after the -e flag that specifies the type of event you want to listen too. The inotify-tools have support for different types of file events such as: create, modify, delete, move, and open. To see the full list and more about the inotify-tools and the inotifywait command you can visit the documentation here: [inotify-wait](https://linux.die.net/man/1/inotifywait)
 

To view additional documentation on the inotify API go here: [inotify linux docs](https://linux.die.net/man/7/inotify)


