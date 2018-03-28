# VScodeDebugGoAppEngine
Hello World tutorial that shows how to setup VS Code to debug Golang App Engine code with Visual Studio (aka VScode )

This is using using the Helloworld code from AppEngine documentation:

   

    go get -u -d github.com/GoogleCloudPlatform/golang-samples/appengine/helloworld/

on a Mac running osX 10.13.3.   

I've tested the code and the server works locally. I'm trying to figure out how to enter into the code with the debugger so I can learn how to use the debugger on other projects.

These were the best instructions I could find for using VScode with GAE but they seem to be outdated based on updates to Golang(e.g.  switch to Gcloud, -go_debugging flag and change of directory structure):  
https://medium.com/@dbenque/debugging-golang-appengine-module-with-visual-studio-code-85b3aa59e0f 

Here are the steps I took:
# set up Environment
- added to .bash_profile  

        export BASEFOLDER="/Users/Bryan/google-cloud-sdk/" . 
        export GOROOT="/usr/local/go" # this shoudln't have to be set with current Version, doing it to follow the tutorial . 

How I have attempted to get debugger to run:  

# start local server .  

    dev_appserver.py --go_debugging=true app.yaml

# attach local binary to Delve

   

     ps aux | grep _go_app 
    
    dlv attach <#using the PID from the server binary>

Delve successfully attaches to the binary.

When I start the **Debug session**, the blue progress bar never stops scanning horizontally, there is no info displayed in the debug window.


Here is the launch.json config:

    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Launch Go Hello World",
                "type": "go",
                "request": "launch",
                "mode": "debug",
                "remotePath": "${workspaceRoot}",
                "port": 2345,
                "host": "127.0.0.1",
                // Removed these because this is not used
                //"port": 2345,
                //"host": "127.0.0.1",
                "program": "${workspaceFolder}/hello.go",
                "env": {},
                "args": [],
                "showLog": true,
                "trace": "verbose"  
            }
        ]
    }

Here are the versions I have installed:  

    go version go1.10 darwin/amd64  
    $ gcloud version . 
    Google Cloud SDK 193.0.0 . 
    app-engine-go . 
    app-engine-python 1.9.67 . 
    bq 2.0.30 . 
    core 2018.03.09 . 
    gcloud . 
    gsutil 4.28 . 
    
    VS code extension:
    Go 0.6.77

