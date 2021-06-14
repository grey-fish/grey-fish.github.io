# Level 4
First time i use Burp to solve a challenge.

## Quest
We are presented with the following webpage. 
![Level 4 Image](./images/Level4.png)

<br/>

## Solution
It clearly mentions that we will be granted password, when we come from a particular URL. In HTTP, the Server uses `Referer` header to find out that which URL has made the request.<br/>
So to solve this level, capture the request in burp, send it to the repeater and add a `Referer` header manually.

![Level 4 Solution](./images/Level4_solution.png)

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
