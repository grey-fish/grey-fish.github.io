# Natas Level 19
We predict "random" `PHPSESSID` to get to next Level

## Quest
We are presented with below page
![](./images/Level19.png)

<br/>

## Solution
As per the message, this level is similar to previous level, with only difference that session ID's are no longer sequential.

In previous level we incremented the session id by 1 (till 640) and somewhere in between we got <span id=green>admin</span> session.

Let's see what cookie is being set here.
![](./images/Level19_solution.png)

Above we see, session ID being set `PHPSESSID=39312d61646d696e`. At first glance, it looks "random".


Lets use Burp Sequencer to collect more of those. We select the POST request and sent to sequencer
![](./images/Level19.1_solution.png)


After we run the sequencer, we end up with lot of tokens, and a pattern begin to emerge
![](./images/Level19.2_solution.png)


Now, i removed the string `2d61646d696e` from all tokens and then sort it and it becomes clear
![](./images/Level19.3_solution.png)

See a pattern, `3#2d61646d696e`, then `3#3#2d61646d696e`, then `3#3#3#2d61646d696e`  (# goes from 0 - 9)


We now make use of burp intruder to brute force all these requests. I got it with the third time. See below
![](./images/Level19.4_solution.png)
Above we can see, we uses the _clusterbomb_ attack type with 3 payloads, each going from 0 to 9.

<br/>
Finally, password for next level is revealed.
![](./images/Level19.5_solution.png)

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
