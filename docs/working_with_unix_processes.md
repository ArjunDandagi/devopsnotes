# Working with unix processes 

## Chapter - 1: Introduction 
Author says , anyone can learn unix process easily :smile: and once you master them
you can map it to anything in application development to how they are behaving
under the hood 


## Chapter - 2 : Primer 

Why care? 
kernel. is a great power , so not directly accessible , 

man pages. 
man pages has sections 

use
```
man man
```
to know more about man pages , and section to select.

```
SECTIONS in man pages: 
• Section 1: General Commands
• Section 2: System Calls
• Section 3: C Library Functions
• Section 4: Special Files
etc

man 3 malloc # will provide how to use library function malloc to allocate memory , its a system call :) 
``` 

process are atoms of unix 
 ```
 ruby -e "p Time.now"  # runs a process prints current time and exits
 ```
 
 


## Chapter 3: Processes Have IDs
1. pids 
2. ppids ,
Ruby : 
```
puts Process.pid 
puts Process.ppid
```

## Chapter 4: Processes Have Parents 
1. pids 
2. ppids ,
Ruby : 
```
puts Process.pid 
puts Process.ppid
```

## Chapter 5 : Processes Have File Descriptors 
FDS as we call them , open file descriptor is misnomer , its just FD .. so there is no close file descriptor , in fact it throws and error
if you try to read from a FD ( file for humans ) that is closed . 

1. FD are tightly coupled to process. 
2. lowest number is used when a file is open 
3. incrementally assigned number 
```
passwd = File.open('/etc/passwd') puts passwd.fileno
hosts = File.open('/etc/hosts') puts hosts.fileno
# Close the open passwd file. The frees up its file descriptor # number to be used by the next opened resource.
passwd.close
null = File.open('/dev/null') puts null.fileno

output:
3
4
3
```
What happened to 0,1,2?
they are STDIN , STDOUT ,STDERR

RUBY : 
```
puts STDIN.fileno 
puts STDOUT.fileno 
puts STDERR.fileno
```


## Chapter 6: Processes Have Resource Limits

everything has limit ( yes , literally everything in computational world has limit :smile: , it might be large , but it is still a limit ) 

RUBY:
```
p Process.getrlimit(:NOFILE)
```
a process can upgrade itself( setrlimit ) if it has the permission to do so 

* soft limit  and hard limits ( soft limits are not real limits , it just throws a exception when you cross that limit , but dont stop you there. 
soft limit)

setting rlimit inside while running it : 
```
Process.setrlimit(:NOFILE, Process.getrlimit(:NOFILE)[1])
```

Exceeding the Limit
Note that exceeding the soft limit will raise Errno::EMFILE :

```
Process.setrlimit(:NOFILE, 3)
File.open('/dev/null')

will output:
Errno::EMFILE: Too many open files - /dev/null
```

## Chapter 7: Processes Have an Environment 

RUBY: 
```
msg="Hello World" ruby -e 'p ENV["msg"]'
```
its not a hash ( python lover? , hash is dictionary in python :wink: )  though , it just allows to access them like a hash.

## Chapter 8: Processes Have Arguments
command line arguments  ( like String [] args in java ) 

RUBY:
```
# ARGV is constant that is an array in ruby 
puts ARGV
```

## Chapter 9: Processes Have Names

RUBY:
```
# you can change it by setting the variable $PROGRAM_NAME 

puts $PROGRAM_NAME
$PROGRAM_NAME = "custom_name"

# now ps will show it as process name as "custom_name". 

```

real world usecase? .. some programs can create additional process name and use it to track things . RESQUE is an example. 


## Chapter 10 : Processes Have Exit Codes 

RUBY:
```
# use these commands one byone 
at_exit { puts "executing last line " }
exit # exits with 0 , u can use exit 88 to return 88 code  --> this will call at_exit upon exit
exit! # exits with 1 , u can use exit 88 , to return 88 as return code --> this will not call at_exit 
abort "hmm somehing went wrong" # will call at_exit
```

## Chapter 11: Processes Can Fork

The child process inherits a copy of all of the memory in use by the parent process, as well as any open file descriptors belonging to the parent process.

```
if fork
 puts "entered the if block #{Process.pid}"
else
 puts "entered the else block #{Process.pid}"
end

## output 
enterd the if block 222 # parent 
entered the else block 223 # why?  --> returns twice ,one in the calling , another in the called 

One call to the fork method actually returns twice. Remember that fork creates a new process. So it returns once in the calling process (parent) and once in the newly created process (child).

```

The call to fork returns near-instantly so we now have 3 processes with each using 500MB of memory. Perfect for when you want to have multiple instances of your application loaded in memory at the same time. Because only one process needs to load the app and forking is fast, this method is faster than loading the app 3 times in separate instances.

The child processes would be free to modify their copy of the memory without affecting what the parent process has in memory ( copy on write )

fork bomb? ( if you fork the same app that takes 500mb 20-40 times , you are fork bombing your system) 

multiple cores? , a fork can not guarantee if the process gets distributed to all the cores . 

using block can solve this 
```
fork do 
## execute anything as part of child process now
end
```

## Chapter 12: Orphaned Processes

```
fork do 
 5.times do
  sleep 1
  puts "I'm an orphan!"
 end
end
abort "Parent process died..."
```
after the control exited also the child continuous to display "I'm an orphan!" :)

so we need to manage child processes properly 

**What happens to a child process when its parent dies?**
The short answer is, nothing. 
That is to say, the operating system doesn't treat child processes any differently than any other processes.
So, when the parent process dies the child process continues on; the parent process does not take the child down with it.

Daemon processes are long running processes that are intentionally orphaned and meant to stay running forever. (systemd unit files ?) 

if a process is not attached to a terminal session how will you kill it? ( by sending signal :smile:  like kill -9 <PID>)
 
 
## Chapter 13: Processes Are Friendly

systems simply dont copy everything in memory when there is fork 

Physically copying all of that data can be considerable overhead,
so modern Unix systems employ something called copy-on-write semantics (CoW) to combat this. ( they dont copy unless the child process wants to mess with it than reading)
CoW (copy on write) delays the actual copying of memory until it needs to be written. (  until one of them needs to modify it. )

copy on write ( forks wont copy everything when you fork child :smile: )
```
 this wont  copy the memory object arr

arr = [1,2,3,4]
fork do 
 p arr
end

while this creates separate copy when we start to make changes 

arr = [1,2,3,4]
fork do 
 arr << 4
end

```

## Chapter 14: Processes Can Wait

fire and forget is good example of unhandled fork , a child can ship logs while the parent can continue to do business logic is one such example . ( for example)

```
fork do 
  5.times do
    sleep 1
    puts "I'm an orphan!"
  end
end

 
# this is just waiting till child completes its execution 
#Process.wait returns the pid of the child that exited
puts Process.wait

abort "Parent process died..."
```
when a child a exits before parent , the kernel ensure the race condition/errors wont happen when parent was busy doing its work 
it queues the exit code and stuff related to child processes. 

FORK is all about unix processes:
At the core of this pattern is the concept that you have one process that forks several child processes, for concurrency, and then spends its time looking after them: making sure they are still responsive, reacting if any of them exit, etc.


## Chapter 15: Zombie Processes

stack overflow :
An **orphan process** is a computer process whose parent process has finished or terminated, though it (child process) remains running itself.
A **zombie process** or defunct process is a process that has completed execution but still has an entry in the process table as its parent process didn't invoke an wait() system call

back to author's words on what a zombie is:
The kernel will retain the status of exited child processes until the parent process requests that status using Process.wait . If the parent never requests the status then the kernel can never reap that status information. So creating fire and forget child processes without collecting their status information is a poor use of kernel resources.

Que: then how can you exit and let the child be an adult and takes care of its own life?
Answer: you need to detach it :smile:
in ruby you can use Process.detach(pid) 

example:
```
message = 'Good Morning'
recipient = 'tree@mybackyard.com'
# In this contrived example the parent process forks a child to take
# care of sending data to the stats collector. Meanwhile the parent
# process has continued on with its work of sending the actual payload.
end
# The parent process doesn't want to be slowed down with this task, and # it doesn't matter if this would fail for some reason.

pid = fork do
 StatsCollector.record message, recipient
end
# This line ensures that the process performing the stats collection
# won't become a zombie.
Process.detach(pid)
```

What does Process.detach do? 
It simply spawns a new thread whose sole job is to wait for the child process specified by pid to exit. 
This ensures that the kernel doesn't hang on to any status information we don't need.

ruby Process.detach doesnt map anything to system call . coz it's implemented in Ruby simply as a thread and Process.wait . 

## Chapter 16: Processes Can Get Signals

Signal delivery is unreliable. By this I mean that if your code is handling a CHLD signal while another child process dies you may or may not receive a second CHLD signal.

Sometimes the timing will be such that things will work out perfectly, and sometimes you'll actually 'miss' an instance of a child process dying.

most likey this happens if there is a burst of child processes that are exiting in succession ( same signal several times in quick succession, you can always count on at least one instance of the signal arriving ) 

To properly handle CHLD you must call Process.wait in a loop and look for as many dead child processes as are available, 
since you may have received multiple CHLD signals since entering the signal handler. But....isn't Process.wait a blocking call?
If there's only one dead child process and I call Process.wait again how will I avoid blocking the whole process?

Process.wait also takes a second argument, flags. One such flag that can be passed tells the kernel not to block if no child has exited. 
Just what we need!
There's a constant that represents the value of this flag, Process::WNOHANG , and it can be used like so:

```
Process.wait(-1, Process::WNOHANG)
```

Process.wait  raise Errno::ECHILD if no child processes exist. So you must handle the Errno::ECHILD exception in your CHLD signal handler.

```
rescue Errno::ECHILD
```

If you don't know how many child processes you are waiting on you should rescue that exception and handle it properly.

> Signals Primer:

**Signals are asynchronous communication.**
When a process receives a signal from the kernel it can do one of the following:
1. ignore the signal
2. perform a specified action
3. perform the default action

> Where do Signals Come From?

Technically signals are sent by the kernel, just like text messages are sent by a cell phone carrier
But text messages have an original sender, and so do signals. 
Signals are sent from one process to another process, using the kernel as a middleman

The original purpose of signals was to specify different ways that a process should be Killed.

```
[vagrant@localhost ch_16]$ kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX
```
you can silently ignore any signal. but SIGKILL is the boss of all signal. that is why, A BOFH ( bastard operator from hell) would use Kill -9 <PID>
 
 **Signal Handlers are Global**
 
trapping Signals in unix are bit like global variables. you might be overwriting a something that someother code depends on . 
 and unlike global variables the signals can not be namespaced. 
 
**Being Nice about Redefining Signals** //TODO , read this again when needed , page 85 ch16. 


In terms of best practices your code probably shouldn't define any signal handlers, unless it's a server. As in a long-running process that's booted from the command line. It's very rare that library code should trap a signal.

```
# the friendly method of trapping a signal
old_handler = trap(:QUIT) { 
# do some cleanup
puts 'All done!'
old_handler.call if old_handler.respond_to?(:call) }
```
This handler for the QUIT signal will preserve any previous QUIT handlers that have been defined. Though this looks 'friendly' it's not generally a good idea. Imagine a scenario where a Ruby server tells its users they can send it a QUIT signal and it will do a graceful shutdown. You tell the users of your library that they can send a QUIT signal and it will draw an ASCII rainbow. Now if a user sends the QUIT signal both handlers will be invoked. This violates the expectations of both libraries.

Whether or not you decide to preserve previously defined signal handlers is up to you,
just make sure you know why you're doing it.
If you simply want to wire up some behaviour to clean up resources before exiting you can use an at_exit hook,

**When Can't You Receive Signals?**

signals are asynchronous , that is how you can stop an infinite sleepy function , a method hogging resources , etc.  that is the power of signals. 

With signals, any process can communicate with any other process on the system, so long as it knows its pid.
In the real world signals are mostly used by long running processes like servers and daemons. And for the most part it will be the human users who are sending signals rather than automated programs

**System Calls**
Ruby's Process.kill maps to kill(2), Kernel#trap maps roughly to sigaction(2). signal(7) is also useful


## Chapter 17: Processes Can Communicate

Inter Process Communication (IPC)
There are many different ways to do IPC but Author covered two commonly useful method: pipes and socket pairs

**Our First Pipe**
A pipe is a uni-directional stream of data. In other words you can open a pipe,
one process can 'claim' one end of it and another process can 'claim' the other end. 
Then data can be passed along the pipe but only in one direction

So if one process 'claims' the position of reader, rather than writer, it will not be able to write to the pipe. And vice versa.

ruby code for creating a pipe 
```
reader, writer = IO.pipe #=> [#<IO:fd 5>, #<IO:fd 6>]
```
IO.pipe returns an array with two elements, both of which are IO objects.

The IO objects returned from IO.pipe can be thought of something like anonymous files. You can basically treat them the same way you would a File . You can call #read ,
#write , #close , etc. But this object won't respond to #path and won't have a location on the filesystem

pipes are one way only.. which means trying to write a read end will throw error (IOError)


**Sharing Pipes**

Pipes are considered a resource, they get their own file descriptors and everything, so they are shared with child processes.

```
reader,writer=IO.pipe

fork do 
	reader.close 

	for i in 1..10 do 
		writer.puts "Message #{i} from child"
	end
end

writer.close

while message = reader.gets
	$stdout.puts message
end
```

as you can see the unused ends are closed so as to  not to interfere with EOF being sent. 
also when a fork there are 4 ends pointing to read and write .. so the unused ends must be closed . 

since pipe are IO objects  we can use IO methods on them , not just read and write , here we have used  puts and gets method. 


**Streams vs. Messages**
When working with an IO stream, like pipes or TCP sockets, you write your data to the stream followed by some protocol-specific delimiter. 
For example, HTTP uses a series of newlines to delimit the headers from the body.

Then when reading data from that IO stream you read it in one chunk at a time, stopping when you come across the delimiter.( That is why puts and gets was used for newline as delimeter)

Unix sockets are a type of socket that can only communicate on the same physical machine. 
As such it's much faster than TCP sockets and is a great fit for IPC.

```
require socket 
Socket.pair(:UNIX,:DGRAM,0)
```
This creates a pair of UNIX sockets these sockets that are already connected up to each other
These sockets communicate using datagrams, rather than a stream. In this way you write a whole message to one of the sockets and read a whole message from the other socket. No delimiters required.


Sockets are used for biderectional transfer.

**Remote IPC?**
If you're interested in scaling up from one machine to many machines while still doing something resembling IPC there are a few things to look into. The first one would simply be to communicate via TCP sockets. 

other solutions like RPC( remote procedural calls) , a message system like ZeroMQ ( aws SQS)  

**REAL WORLD**
many child processes communicate over a single pipe with their parent process.

**System Calls**
Ruby's IO.pipe maps to pipe(2), Socket.pair maps to socketpair(2). Socket.recv maps to recv(2) and Socket.send maps to send(2).


 
 
 




















