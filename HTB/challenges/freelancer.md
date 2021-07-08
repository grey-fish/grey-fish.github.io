# Freelancer
>Name : Freelancer<br/>
>tags : web<br/>
>Solved: 06 July 2021<br/>

## Intro
A message from creater :  _"Can you test how secure my website is? Prove me wrong and capture the flag!"_

<br/>

## Solution
We are presented with a website, Lets run `gobuster` on it.


We find following interesting directories

  - `/administrat`  : presents a login form<br/>
  - `/mail`      : contains `contact_me.php` file which yields 500 Internal error.<br/>

Analyzing the source for main page reveals a link `portfolio.php?id=1`.

We run `sqlmap` and see that parameter `id` is injectible.

![](./images/freelancer2.png)

<br/>

Alright, we extract the Database name  : `freelancer` below

![Image3](./images/freelancer3.png)

<br/>

Dumping tables of DB `freelancer`

![](./images/freelancer4.png)

<br/>

The `safeadmin` table looks interesting, lets dump its content.

![](./images/freelancer5.png)

Above we extracted the password hash, but i was unable to break it. So moving on.

<br/>

Moving ahead, while further bruteforing the `/administrat` directory, `panel.php` was found

![](./images/freelancer6.png)

<br/>

When we try to access `panel.php`, we are redirected to the index page. Lets read it's code using `sqlmap`

![](./images/freelancer7.png)

<br/>

In the `panel.php`, we find our <span id=green>flag.</span>

![](./images/freelancer8.png)

That is the challenge !

<br/>

[<< Back](https://grey-fish.github.io/HTB/index.html)

