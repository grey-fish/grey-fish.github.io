# Bandit

## Level 30
There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

<br/>
## Solution

We clone the repository in `/tmp` directory. 

Like in previous level, we check below things, but no password:<br/>

  - Inspecting files via `git log` does give us password<br/>
  - There are no remote branches configured.<br/>


Inspecting some more, we see that there is a git tag present named `secret`. Inspecting the tag using `git show` reveals our password!.

Solution Screenshot:

![Level 30 Image](./images/Level30)


<br/>

[<< Back](https://grey-fish.github.io/Bandit/index.html)

















