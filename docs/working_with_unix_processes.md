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
FDS as we call them , open file descriptor is misnomer , its just FD 

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







