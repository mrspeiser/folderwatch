## Folderwatch

##### A simple application that listens to a folder and executes an event script on the creation of a file

<br />
**Requirements:**
1. Linux system with bash
2. inotify-tools
```apt-get -y install inotify-tools```

<br />
**Installation**
1. ```cd /etc/```
2. ```git clone https://github.com/mrspeiser/folderwatch.git```

I recommend cloning the repository into /etc/ because /etc/ is generally for etcetera application and will be easy to access for starting a listener from any working directory.

<br />
**Startup**

1. Navigate to the directory you want to listen in

  ```cd /home/drop/myspecialfolder```

2. Initialize the pipe and hidden directory for the output of the inotify tools:
  
```/etc/folderwatch/inotify-event-listener```
   - You can also pass the name of the pipe as the first argument, otherwise it will prompt you 
```/etc/folderwatch/inotify-event-listener mypipename```

3. Enter the hidden directory 
  
```cd /home/drop/myspecialfolder/.folderwatch```

4. Start the listener to forward the output from the pipe to the event
  
```./startListener```
  
   - You can also pass the pipename as the first parameter  
```./startListener pipename```
  
   - Additionally you can also manually start the listening process by executing the listner as a background process. startListner script is just a shortcut 
```./listener "$pipename" &```

<br />
**Verifying Startup/Running**

There are 2 simple useful scripts that get copied into the hidden folder to view and filter processes:
1. ps_inotify
2. ps_listener

The **ps_inotify** will show you all the inotify processes currently running. If you do not see the folder that you started the process in, you will need to delete the hidden folder and start over.

The **ps_listener** will show you all the named-pipe processes currently running. If you do not see the pipe running you will need to start the listner again by running the **startListner** script inside the hidden folder.

<br />
**The Event Script**

The event script fires everytime the inotify-process outputs to the pipe. The pipe feeds that output into the event.

The default behavior for the event script is to log the output from the inotify-process to the log.txt file inside the .folderwatch folder.

The output to the log will look something like this:  

inotify output: CREATE,THIS-FILE-WAS-JUST-CREATED.txt    --    timestamp: Fri Apr  5 19:15:22 UTC 2019

** You _DO NOT_ need to stop the listener or any processes to modify the event script **

The event script runs independently and only gets triggered when it receives input from the pipe.

