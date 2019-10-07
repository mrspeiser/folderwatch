## Folderwatch
##### A simple application that listens to a folder and executes an event script on the creation of a file

**Requirements:**
1. Linux system with bash
2. inotify-tools
```apt-get -y install inotify-tools```

**Installation**
1. ```cd /etc/```
2. ```git clone https://github.com/mrspeiser/folderwatch.git```

I recommend cloning the repository into /etc/ because /etc/ is generally for etcetera application and will be easy to access for starting a listener from any working directory.

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

