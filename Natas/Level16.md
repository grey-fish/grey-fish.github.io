# Natas Level 16
This level builds on Level 10, and changes the backend code so that it becomes more secure.

## Quest
We are presented with a webpage similar to what we saw in Level 10.

![Level 16 Image](./images/Level16.png)

Here is the backend code, i'll highlight the differences.
```php
<pre>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&`\'"]/',$key)) {     // Dont allow ; | ' " ` & in user input
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i \"$key\" dictionary.txt");  // Our input is inserted in quotes now
    }
}
?>
</pre>
```

## Solution
Above i've highlighted two main differences in the code, here they are again:<br/>
  - Dont allow `;`, `|`, `'`, `&` `"` and backtick character in user input<br/>
  - Our input is now enclosed in quotes `"`
Allowed characters that can be helpful are `$^({`. We will use them to get the password.

> Now before we proceed, i must say that after completing this writeup, i'll look out to see how other people did it because i certainly took the hard way and there is bound to be a simpler approach to this. Nonetheless, i did it and i want to document how it was done.

Now, looking at the PHP code, it seemed that this one would be easy like Level 10, but this took more time.

Our input is placed inside `"` in PHP code and i found no way to escape it, so only thing left was to make best of the condition and smuggle data slowly while remaining inside `"`.

First, we check what we can do,  we can search for `^w` and `^w$` and it works just fine. See below
![Level 16 solution](./images/Level16_solution.png)

As backticks are allowed, we check if we can run a command using `$()`. We enter `^$(echo)`. This essentially converts to below command:
`grep -i "^$(echo H)" dictionary.txt`  _// all words starting with letter H_
![Level 16 solution](./images/Level16.1_solution.png)

<br/>

> Now we have a rough strategy, read the the password file character by character, that character read will be our search term which in turn will then be first character of our output.

Also, lets figure out how to display particular character from a file. We use `cut` command, see below:
```shell
# Sample file
$ cat test.txt
Hello World!
# Get First character
$ cut -c 1 test.txt
H
# Get 12th character, and so on
$ cut -c 12 test.txt                                                                                   
!
```

Now, this will work but we have a problem, actually we have 2 problems:
  i. Our password can contain numbers, which are not present in dictinary words. How do we extract them ?
  ii. How will we differentiate between lowercase and uppercase letters as `grep` uses `-i` flag ?
  
 We'll address them, first lets work with what we have in hand, and extract what we could.
 We use this payload ->  `^$(cut -c 1 /etc/natas_webpass/natas17)`

First character            |  Second character
:-------------------------:|:-------------------------:
![](./images/Level16.2_solution.png)  |  ![](./images/Level16.3_solution.png)

We repeat above process again and again. We put a `.` when we get no output and First letter, when we get on output. 
Throwing some more screenshots here.
Thirteenth character       |  Thirtieth character
:-------------------------:|:-------------------------:
![](./images/Level16.4_solution.png)  |  ![](./images/Level16.5_solution.png)

Now
