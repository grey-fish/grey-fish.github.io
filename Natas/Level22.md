# Natas
> Level : Natas Level 22<br/>
> Solved : 13th July 2021<br/>
> Remarks : This was Easy Peasy<br/>
<br/>

## Quest
We are presented with below webpages
![](./images/Level22.png)

Relevant Backend code
```php
<?
    if(array_key_exists("revelio", $_GET)) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas23\n";
    print "Password: <censored></pre>";
    }
?>
```

## Solution

This one was surprisingly easy, from the above code we can see that password will be revealed if we send a parameter `revelio` in the GET request.

Below we do that by adding `?revelio=true` and it reveals the password.

![](./images/Level22_solution.png)


This was a quick one !

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
