# Bandit

## Level 29

There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

<br/>
## Solution

Create a directory in `/tmp` and clone the repo there. Checked `git log` as well, no password found. 

Screenshot:

![Level 29 Image](./images/Level29.png)


After further playing and digging, i found that there is a remote branch `dev` added to it. 
Note: Remote branches are visible in `git branch -a` and not in `git branch` which lists only local branches.

So, switching to the remote branch and we found password in README file.

Solution Screenshot:

![Level 29 Image](./images/Level29.1.png)

<br/>

[<< Back](https://grey-fish.github.io/Bandit/index.html)






















