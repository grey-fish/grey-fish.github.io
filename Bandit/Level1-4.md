# Bandit

## Level 1
Quest: The password for the next level is stored in a file called readme located in the home directory.
<br/>
## Solution 1
Log into Level 1, we see a file named `-` . Since `-` also represents `stdin` in terminal so `cat -` doesn't work.<br/>
In order to view the file, we use absolute path or relative path as shown below.

![Level 1 Image](./images/Level1.png)

<br/>
<br/>

## Level 2
Quest: The password for the next level is stored in a file called spaces in this filename located in the home directory
<br/>
## Solution 2

Password for next level is in a file with spaces in its name. Its super easy, we use tab completion. Just type first letter of the file and press <u>tab</u> and name autocompletes.<br/>

See Below:
![Level 2 Image](./images/Level2.png)

<br/>
<br/>

## level 3
Quest: The password for the next level is stored in a hidden file in the inhere directory.
<br/>
## Solution 3
It's simple, we use `ls` command with `-a` flag to reveal hidden files as shown below

![Level 3 Image](./images/Level3.png)

<br/>
<br/>

## Level 4
Quest: The password for the next level is stored in the only human-readable file in the inhere directory
<br/>
## Solution 4


<br/>
<br/>

[<< Back](https://grey-fish.github.io/Bandit/index.html)
