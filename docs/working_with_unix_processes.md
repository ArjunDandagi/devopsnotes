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








