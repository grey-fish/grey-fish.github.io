# Bandit

## Level 18
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

<br/>
## Solution
This seems interesting. As stated above, when we login using our credentials, we are immediately logged out.

We can solve this level by providing a <u>command</u> while logging in via SSH

From the Docs:
> If a command is specified, it is executed on the remote host instead of a login shell

Final Command:
```shell
