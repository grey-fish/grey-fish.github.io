Created : 12th May
tags : #python 🐍 

# Python Virtual Environments
It began when i tried to install `altdns`  in Kali but couldn't as it relied on Python 2 (even though Kali defaults to python2). Naturally, i wanted a permanent fix for this problem. 

## What is the Problem?
I want to be able to run / install a python program irrespective of what python version is installed on my pc. 

## Solution
We use `pyenv` to use different versions of python along with virtual environments options for each project. 

## Explain in Detail
First we look at how to install pyenv and then go to my workflow.

#### Installing pyenv
1.Installing Python ( For ubuntu or Kali )

```shell
kali@kali:~$ sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python3-openssl git

kali@kali:~$ curl https://pyenv.run | bash
```
<br/>

2.Depending upon the shell, add following lines to your `.bashrc` or `.zshrc`

```shell
# Here my default shell is zsh
kali@kali:~$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc 
kali@kali:~$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc 
kali@kali:~$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init --path)"\nfi' >> ~/.zshrc         # Note the slight change here
kali@kali:~$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```
<br/>

3.Restart the Shell

```shell
kali@kali:~$ exec $SHELL
```
Note: Restart if using GUI or log off and log in again if using GUI
<br/>

Now we have pyenv installed. We can check via `pyenv versions`

```shell
kali@kali:~$ pyenv versions
* system (set by /home/ubuntu/.pyenv/version)
  2.7.18
  
 # Install python 2.7.18 using below command
 kali@kali:~$ pyenv install 2.7.18       # Above we can see already done, so not installing here again
 ...
```

Now we have two options, first is to switch python version globally from system default (Python3) to Python 2.7.18 (that we installed now), but we won't go to that route.
Instead we will make virtualenvs, that use Python 2. 

## My Workflow
After Setting up `pyenv` and installing the desired python version. Lets make different version for different projects with help of `virtuaenv`.
I'll explain workflow with help of a use case. 

Suppose i want to install `altdns` (requires python2). So i do the following

```shell
# Here i start with a fresh VM
kali@kali:~$ pyenv versions
* system (set by /home/kali/.pyenv/version)

kali@kali:~$ pyenv install 2.7.18 
Downloading Python-2.7.18.tar.xz...
-> https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tar.xz
Installing Python-2.7.18...
Installed Python-2.7.18 to /home/kali/.pyenv/versions/2.7.18

kali@kali:~$ pyenv versions
* system (set by /home/kali/.pyenv/version)
  2.7.18

kali@kali:~$ mkdir tools                             
kali@kali:~$ cd tools

# Create project folder
kali@kali:~$ mkdir altdns && cd altdns

# Activate pyenv-virtualenv
kali@kali:~$ pyenv virtualenv 2.7.18 venv-altdns
kali@kali:~$ pyenv local venv-altdns && pyenv activate venv-altdns

(venv-altdns) kali@kali:~$       
# Changed directory and prompt goes away
kali@kali:~$ cd altdns 

# vevn automatically acitvated when chagned to the directory
(venv-altdns)  kali@kali:~$ pip install py-altdns  
...
# altdns installed and works
(venv-altdns) kali@kali:~$ altdns -h            
usage: altdns [-h] -i INPUT -o OUTPUT [-w WORDLIST] [-r] [-n] [-e]
              [-d DNSSERVER] [-s SAVE] [-t THREADS]

...
```
<br/>

## What we Achieved
Automatic switching between different python versions (_depending on our current working directory_). <br/>
Meanwhile, Virtualenvs (_serving as isolated containers_) give us power to use different python versions without conflicting with eachother. 

<br/>

[<< Back](https://grey-fish.github.io/Misc/)
