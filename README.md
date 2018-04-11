# VScodeDebugGoAppEngine
Hello World tutorial that shows how to setup VS Code to debug Golang App Engine code with Visual Studio (aka VScode )

This is using using the Helloworld code from AppEngine documentation:

    go get -u -d github.com/directmeasure/VScodeDebugGoAppEngine.git

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


The Breakpoint is set at hello.go: line 21

# attach local binary to Delve  

$ ps aux | grep _go_app

    Bryan             653   0.0  0.0  2434840    800 s000  S+    5:00PM   0:00.00 grep _go_app
    Bryan             649   0.0  0.0 556601764   3804 s002  S+    5:00PM   0:00.01 /var/folders/mw/0y88j8_54bjc93d_lg3120qw0000gp/T/tmp9SrojIappengine-go-bin/_go_app

Bryan@Bryans-MacBook-Pro Tue Apr 10 17:00:38 ~ 
$ dlv attach --headless -l "localhost:2345" 649 

    /var/folders/mw/0y88j8_54bjc93d_lg3120qw0000gp/T/tmp9SrojIappengine-go-bin/_go_app
    API server listening at: 127.0.0.1:2345


**Starting Debug Session:**  
https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code#remote-debugging  
Above docs suggest that the port should be the same as assigned to the headless Delve server set above "2345"

When "port":"2345" is the same as assigned port headless Delve server. 
Debugger REPL window:

    Verbose logs are written to:
    /var/folders/mw/0y88j8_54bjc93d_lg3120qw0000gp/T/vscode-go-debug.txt
    couldn't start listener: listen tcp 127.0.0.1:2345: bind: address already in use
    Process exiting with code: 1

When "port": "1234", REPL window shows:

    Verbose logs are written to:
    /var/folders/mw/0y88j8_54bjc93d_lg3120qw0000gp/T/vscode-go-debug.txt
    2018/04/11 10:25:06 server.go:73: Using API v1
    2018/04/11 10:25:06 debugger.go:98: launching process with args: [/Users/Bryan/Dropbox/go/src/helloworld/debug]
    API server listening at: 127.0.0.1:1234
    2018/04/11 10:25:07 debugger.go:340: created breakpoint: &api.Breakpoint{ID:1, Name:"", Addr:0x20c4, File:"/Users/Bryan/Dropbox/go/src/helloworld/hello.go", Line:21, FunctionName:"main.handle", Cond:"", Tracepoint:false, Goroutine:false, Stacktrace:0, Variables:[]string(nil), LoadArgs:(*api.LoadConfig)(nil), LoadLocals:(*api.LoadConfig)(nil), HitCount:map[string]uint64{}, TotalHitCount:0x0}
    2018/04/11 10:25:07 debugger.go:497: continuing
    
The VARIABLE sidebar in Debug mode is never populated with the variables in hello.go


Here are the versions I have installed:  

        go version go1.10 darwin/amd64  
        $ gcloud version . 
        Google Cloud SDK 197.0.0
        app-engine-go 
        app-engine-python 1.9.68
        bq 2.0.31
        core 2018.04.06
        gsutil 4.30

        VS code extension:
        Go 0.6.78