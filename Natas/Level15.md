# Level 15
Pushing things further, here we do Blind SQL injection. Also, we'll make a python script to do the same.

##  Quest
We are presented with a simple page that checks if a user exists or not.
![Level 15 Image](./images/Level15.png)

Backend code is similar to previous level with some important changes
```php
<?
/*
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
*/

if(array_key_exists("username", $_REQUEST)) {
    $link = mysql_connect('localhost', 'natas15', '<censored>');
    mysql_select_db('natas15', $link);
    
    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysql_query($query, $link);
    if($res) {
    if(mysql_num_rows($res) > 0) {
        echo "This user exists.<br>";
    } else {
        echo "This user doesn't exist.<br>";
    }
    } else {
        echo "Error in query.<br>";
    }

    mysql_close($link);
} else {
?>
```
<br/><br/>
## Solution
From the source code, we can see that<br/>
 - Again unsanitized user input is inserted into query.<br/>
 - Although no output is displayed after query execution, we can make use of blind SQL injection.<br/>
 - When query succeeds, we get _user exists_, and when it fails, we get _user doesn't exist_ msg. we'll leverage this behaviour<br/>
 - Additionally, we can see that there is a debug parameter, which displays query on screen.<br/>
 - Lastly, there is a users table with username and password column.<br/>

