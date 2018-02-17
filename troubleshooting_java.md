# Using Java's troubleshooting tools in Linux

## How to run commands
You need to use the same user (tomcat in this example) or belong to the same group that has started the JVM process. You can do that by either using/changing to the actual user:

`su - tomcat`

or alternatively you can use sudo to run the command as that user:

`sudo runuser -l tomcat -c "whoami"`

You can find out the user that has launched JVM by typing this:

`ls -la /tmp/.java_pid<pid>`

## Dump JVM info
This dumps all necessary JVM info out of the JVM process:

`sudo runuser -l tomcat -c "jinfo <pid>"`

## Create a heap dump
You simple run the command and specify the jmap parameters as you like. For example:

`sudo runuser -l tomcat -c "jmap -dump:live,format=b,file=heap.bin <pid>"`

You can use jhat to analyze the heap. I'd advise you to download the dump file to your local computer. You can then run jhat:
```
MyMacBook:Desktop suolapeikko$ jhat heap.bin 
Reading from heap.bin...
Dump file created Fri Oct 27 21:52:13 EEST 2017
Snapshot read, resolving...
Resolving 2762481 objects...
Chasing references, expect 552 dots........................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
Eliminating duplicate references........................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
Snapshot resolved.
Started HTTP server on port 7000
Server is ready.
```
After jhat has processed the binary dump file from the example above, you can open `localhost:7000` on your browser to start investigating the heap.

## Create a live object histogram

You can just type:

`sudo runuser -l tomcat -c "jmap -histo:live <pid>"`

## Monitor garbage collection using jstat
You can monitor garbage collection using jstat tool. For example, to gather five samples in 200ms intervals, you can use:

`sudo runuser -l tomcat -c "jstat -gcutil <pid> 200 5"`

Other example, gather hundred garbage collection stats and causes in 2000ms intervals:
`sudo runuser -l tomcat -c "jstat -gccause 47092 2000 100"`
 
## Run JVM commands
You can run different commands using jcmp. For list of command interfaces, you can use:

`sudo runuser -l tomcat -c "jcmd <pid> help"`

As an example, this command tells JVM to run garbage collection:

`sudo runuser -l tomcat -c "jcmd <pid> GC.run"`

## Dump stack trace
The best way to understand what JEE or other Java Web Container is doing with it's thread is to dump stack trace and analyze thread statuses. You can do that by typing:

`sudo runuser -l tomcat -c "jstack <pid>"`

Look especially for `WAITING (on object monitor)` for concurrency problems. `RUNNABLE` threads are doing something actively, so by looking at the stack you can basically see what they are doing. For example, lots of runnables in database libraries might point to a considerable amount of time to processing and iterating database queries.
