# Freelancer
>Name : Freelancer<br/>
>tags : web<br/>
>Solved: 06 July 2021<br/>

## Intro
Web challenge with a message from creater :  _"Can you test how secure my website is? Prove me wrong and capture the flag!"_

## Solution
We are presented with a website, Lets run `gobuster` on it.


We find following interesting directories

- `/administrat`  : presents a login form
- `/mail`         : contains `contact_me.php` file which yields 500 Internal error.

Analyzing the source for main page reveals a link `portfolio.php?id=1`.
