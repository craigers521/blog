---
title: Ansible and Libssh on Apple Silicon
header:
  teaser: /assets/images/post6/ansible.png
  excerpt: Getting ssh connections to work with ansible on apple silicon macbooks
description: Migrating from paramiko to ansible-pylibssh I was getting import errors, this is the solution
tags: [python, ansible, ssh]
---
![ansible logo]({{ site.url }}{{ site.baseurl }}/assets/images/post6/ansible.png)  

#### The Problem

If you are using ansible to configure network equipment you likely still need to connect to those devices via ssh.  Ansible support for paramiko is waning these days and its recommended to use libssh libraries made usable for python via the ansible-pylibssh module.  I was having some trouble getting this to work and getting consistent import errors despite the module being installed via pip.  

```
"msg": "Failed to import the required Python library (ansible-pylibssh)
```

#### The Solution

Here's what worked to resolve the issue for me on my apple silicon M1 macbook.  

First let's clean up the old module: 

```
pip uninstall ansible-pylibssh
pip cache purge
```

Now let's make sure we have the right dependencies installed via brew:

```
brew install libssh libssh2
```

Now here is the tricky part, we need to make our own python wheel and make sure it knows where to find the libraries brew installed.  That can be done via this command: 

```
CFLAGS="-I $(brew --prefix)/include -I ext -L $(brew --prefix)/lib -lssh" pip install --no-binary :all: ansible-pylibssh
```

I'm using pyenv with virtualenvs, I tested this process on python 3.10, 3.11 and 3.12 and its working for all of them, I am able to successfully ssh connect to my cisco devices using the latest ansible 2.16 core.  


#### Bonus Section

**Watch out for this totally misleading error**
{: .notice--warning}
```
fatal: [c9000v-sw1]: FAILED! => {"changed": false, "msg": "ssh connection failed: Failed to authenticate public key: The key algorithm 'ssh-rsa' is not allowed to be used by PUBLICKEY_ACCEPTED_TYPES configuration option"
```
  

This is actually a result of an authentication failure.  I forgot that I was exporting my credential via an environment variable and when I restarted my terminal I lost those temp env vars. 

**Another Pro Tip**
{: .notice--info} 

I noticed through this process that brew was independently installing its own copies of python to meet its own dependencies.  I would prefer to have pyenv manage all the python versions on my machine. You can resolve this by symlinking the pyenv bins into brew.  Here's a handy script for that.  

```sh
#!/bin/bash

pyenv-brew-relink() {
    rm -f "$HOME/.pyenv/versions/*-brew"
    for i in $(brew --cellar)/python* ; do
      for p in $i/*; do
        echo $p
        ln -s -f $p $HOME/.pyenv/versions/${p##/*/}-brew
      done
    done
    pyenv rehash
}
```

