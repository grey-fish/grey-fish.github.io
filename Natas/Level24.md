# Natas
> Level : Natas Level 24
> Solved : 22nd July 2021
> Remarks : Errors are good

# Quest
We are presented with below webpage

![](./images/Level24.png)

Relevant Backend Code

```php
<?php
    if(array_key_exists("passwd",$_REQUEST)){
        if(!strcmp($_REQUEST["passwd"],"<censored>")){
            echo "<br>The credentials for the next level are:<br>";
            echo "<pre>Username: natas25 Password: <censored></pre>";
        }
        else{
            echo "<br>Wrong!<br>";
        }
    }
    // morla / 10111
?> 
```

## Solution
Above code use strcmp function. Which returns `0` when provided two strings are identical, `!0` is 1 , which is `True`, then password for next Level is revealed.

So initially it seems, we need to know the `<censored>` password, to crack this. (which we can't know, it can be anything!)

While reading the documentation, in user contributed notes, i stumbled upon this
> Since it may not be obvious to some people, please note that there is another possible return value for this function.
> strcmp() will return NULL on failure.

This seems interesting because in PHP:

```php
echo (!NULL);     // Outputs 1

# Which is True
```

So our task is to somehow cause an error when the line `if(!strcmp($_REQUEST["passwd"],"<censored>")){` executes and we'll be golden.

I tried many things, but in the end what succedded was `passwd[]=hello` payload.

![](./images/Level24_solution.png)


We have solved the level, just out of curiosity, i wanted to see the contents of `$_REQUEST` superglobal after our payload. 
So i made a simple PHP page to output just that, below is the output if someone wants to know how this works.

![](./images/Level24.1_solution.png)

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
