# Level 5
We need to analyze the HTTP response in order to succeed in this level

## Quest
We are presented with a webpage with a simple message "_Access disallowed. You are not logged in_" as shown below
![Level 5 Image](./images/Level5.png)

<br/>

## Solution
Below is the request and response in Burp

![Level 5 Solution](./images/Level5_solution.png)

<br/>

Notice the Header in the response : `Set-Cookie: loggedin=0`. The `Set-Cookie` header is used to set the cookies that will be sent in the subsequent requests.<br/>
Perhaps this can help us if we change its value to `loggedin=1` indicating success (_means we have already logged in successfully_). 

![Level 5.1 Solution](./images/Level5.1_solution.png)
<br/>

And Indeed, we can see above that this worked, and we have the password for next Level.

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
