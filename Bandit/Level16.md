# Bandit

## Level 16
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

<br/>
## Solution
Ran `nmap` on localhost and found two ports using SSL as shown below:

![Level 16 Image](./images/Level16.1.png)

<br/>
No luck on port 31518. Port 31790 gives us a private SSH key (hopefully for bandit17).

![Level 16 Image](./images/Level16.2.png)

<br/>
Coped the key to file named `bandit17`, set appropriate permissions and try to login as bandit17. 

![Level 16 Image](./images/Level16.3.png)

This works!

<br/>

[<< Back](https://grey-fish.github.io/Bandit/index.html)
