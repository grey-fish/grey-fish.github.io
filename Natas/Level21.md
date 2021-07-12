# Natas
> Level : Natas Level 21<br/>
> Solved : 12th July 2021<br/>
> Remarks : Dealing in Cookies<br/>

## Quest
We are presented with below webpages
![](./images/Level21.png)

The Backend code on First page is similar to previous level, it consists of `print_ceredentials` function, which reveals the password if `$_SESSION[admin] == 1`.
Below is code for Second Page. Lets comment it
```php
<?  
session_start();

// if update was submitted, store it        // BAD Practise
if(array_key_exists("submit", $_REQUEST)) { // Take values from POST request
    foreach($_REQUEST as $key => $val) {    // +and append to $_SESSION array
    $_SESSION[$key] = $val;
    }
}

if(array_key_exists("debug", $_GET)) {    // Print DEBUG Information
    print "[DEBUG] Session contents:<br>";
    print_r($_SESSION);
}

// only allow these keys
$validkeys = array("align" => "center", "fontsize" => "100%", "bgcolor" => "yellow");
$form = "";

$form .= '<form action="index.php" method="POST">'; 
foreach($validkeys as $key => $defval) {
    $val = $defval;
    if(array_key_exists($key, $_SESSION)) {
    $val = $_SESSION[$key];
    } else {
    $_SESSION[$key] = $val;
    }
    $form .= "$key: <input name='$key' value='$val' /><br>";
}
$form .= '<input type="submit" name="submit" value="Update" />';
$form .= '</form>';

$style = "background-color: ".$_SESSION["bgcolor"]."; text-align: ".$_SESSION["align"]."; font-size: ".$_SESSION["fontsize"].";";
$example = "<div style='$style'>Hello world!</div>";

?>

<p>Example:</p>
<?=$example?>
```

<br/>
<br/>

## Solution

Reading the First section of code on Second page gives us our first hint. Below is the Bad Code
```php
if(array_key_exists("submit", $_REQUEST)) { // Take values from POST request
    foreach($_REQUEST as $key => $val) {    // +and append to $_SESSION array
    $_SESSION[$key] = $val;
    }
}
```
Above code appends whatever is in the POST Body to `$_SESSION` array. BAD Practise !

Lets exploit this. we know to solve the level, we need to add key `admin` with value `1` to `$_SESSION` array.

Below is a POST request with `admin=1` added to body. Optionally `PHPSESSID` has also been changed to identify the session.
We can see in the output that our payload has been added to `$_SESSION` array.
![](./images/Level21.1_solution.png)


Now, our session, identified by `PHPSESSID=yabbadabbado` had `$_SESSION[admin]` set to `1`.

So we simply send this request to webpage when with the same Cookie, which reveals password for next Level.
![](./images/Level21.2_solution.png)

<br/>
This was relatively easy one.

<span id=green>**Takeaway**</span><br/>

  - Read the documentation for `session_start()` function

<br/>

[<< Back](https://grey-fish.github.io/Natas/index.html)