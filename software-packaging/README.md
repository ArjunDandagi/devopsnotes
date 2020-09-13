# pip 

packaging for python application
you can use twine to upload , need to have account in pypi.org , the package name is universal . test.pypi.org for testing purpose

Have developed a pip package named 'stupidrealbash'
`pip install stupidrealbash` 
[Pip Link](https://pypi.org/project/stupidrealbash/)


# ruby gem 
 packaging for ruby application/modules
 
 have developed a simple gem 
 `gem install arjun` 
 or 
 `gem download arjun`
 [Gem Link](https://rubygems.org/gems/arjun)
 
 
 # Brew , 
 taps are nothing but the github repo/ or the common place where you keep your formulas 
 mine is now at https://github.com/arjundandagi/homebrew-brew
 
 so you can tap it first and then pull the brew formula you need 
 using 
 `brew install dirtouch` <-- my version of dirtouch 
 
 # apt 
 [Joe Collins]() video explain about how to package it as .deb file and distribute and install using `dpkg -i file.deb` command 
 
 The tree structure should be something like this for that . also note about the root directory. it is `file_version_os_arch`
```
dirtouch_1.0.0_linux_amd64
├── DEBIAN
│   └── control
└── usr
    └── bin
        └── dirtouch

```
 contents of `dirtouch` shell script are like this 
 ```
 #!/bin/bash

if [ $# -eq 0 ];
then 
  echo "Usage (v1.0.0):"
  echo "     dirtouch FOLDER/PATH/files"
  echo " "
  echo "example:"
  echo "dirtouch foo/bar/{1..9}.txt -- will create two folders and files through 1 to 9.txt inside foo/bar folder"
  echo " "
  exit 1;
fi

FOLDER=$(echo "$@" | xargs -n 1 | rev | cut -d "/" -f2- | rev | sort | uniq | xargs -n 1 mkdir -p )
touch $@
 ```
 
 launchpad wont accept a .deb file directly.. so need to work on that yet.
 
 //TODO: create a package in launchpad to install a package using apt
