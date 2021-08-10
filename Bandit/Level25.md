# Bandit

## Level 25
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

<br/>
## Solution
I vividly remember even after an year, (coz i failed to solve) how this Level astonished me. Personally for me this Level is the best one in Bandit and embraces the Hacker Spirit.

We Log in as user bandit25, and we see a private SSH key for bandit26 user. So we login using the private SSH key as user bandit26, and we are displayed a text and logged out.

Q: Why this happends? 
A: Default shell for bandit26 is changed to a custom shell script. See Below:
```shell
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext          <----
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```
