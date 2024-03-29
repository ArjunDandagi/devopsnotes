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
 1. create account in launchpad 
 2. generate gpg key 
 3. upload to keyserver.ubuntu.com
 3. sign code of conduct ( using gpg) 
 all the above steps are mentioned [here](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Uploading_the_key_to_Ubuntu_keyserver)
 
 https://askubuntu.com/questions/71510/how-do-i-create-a-ppa
 
 once the signing is done , create a ppa .. mine is `ppa:arjundandagi/dirtouch`
 
 [Joe Collins](https://youtu.be/ep88vVfzDAo) video explain about how to package it as .deb file and distribute and install using `dpkg -i file.deb` command 
 
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
 
 ```bash
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
 
 ?> launchpad wont accept a .deb file directly.. so need to work on that yet.
 
 I have tried doing the following as well : 
 [Stack overflow](https://askubuntu.com/questions/27715/create-a-deb-package-from-scripts-or-binaries/27731#27731)
 [for python packaged as apt](https://askubuntu.com/questions/90764/how-do-i-create-a-deb-package-for-a-single-python-script)
 
 //TODO: create a package in launchpad to install a package using apt
 
# maven 
[Official sonatype resources](https://central.sonatype.org/pages/ossrh-guide.html)
[jenkov quick tip to deploy ](http://tutorials.jenkov.com/maven/publish-to-central-maven-repository.html)

my package is now at [MAVEN](https://repo.maven.apache.org/maven2/io/github/arjundandagi/topic-application/)
read about it in [GITHUB](https://github.com/arjundandagi/topic-application)


# npm
 
 > prints-yourname

Prints yourname , in to the console 

## USAGE:

install the package using command 

```
npm i prints-yourname
```


you can use the package like this
```
$ echo "require('prints-yourname');" > index.js
$ node index.js
```

## HOW?

1. create a folder ( prints-yourname) //its already taken now , so use a different one 
2. cd <folder>
3. npm init ( and fill those details )
4. touch index.js 
5. write stuff inside index.js ( i just wrote console.log("<secret>");
6. npm login 
7. npm publish 

The only requirement to publish to npm registry is  package.json . how cool is that . 

I have learnt packaging for Brew , pip , apt ... but this is the one i really liked :) 

[Link to my npm package](https://www.npmjs.com/package/prints-yourname)





 
 
