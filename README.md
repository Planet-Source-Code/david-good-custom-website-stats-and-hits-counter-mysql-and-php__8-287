<div align="center">

## Custom website Stats and Hits counter \- MySQL and PHP


</div>

### Description

Custom code records basic statistics (IP, Host, Referrer, and UserAgent) and hits count for unique visitors to your website. Tries to prevent duplicate entries.
 
### More Info
 
ASSUMES:

//MySQL is installed and configured

//PHP is installed as an Apache Module

//You have access to Apache environment variables


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[David Good](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/david-good.md)
**Level**          |Beginner
**User Rating**    |4.3 (26 globes from 6 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Internet/ Browsers/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-browsers-html__8-9.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/david-good-custom-website-stats-and-hits-counter-mysql-and-php__8-287/archive/master.zip)

### API Declarations

```
// David Good - 2000
// clannagael@gmail.com
// Feel free to use and/or modify
// this code however you see fit.
```


### Source Code

```
// REQUIRED: 2 separate tables:
// Create table statements:
// CREATE TABLE hits(count int not null default 0);
// CREATE TABLE stats(id int Primary Key Not null default 0 auto_increment, ipaddress varchar(15), host varchar(150), browser varchar(100), referer varchar(200));
//For demonstration purposes we'll assume the
//database is called Statistics, You're
//connecting from localhost with a user id
//"user" and a password of "password"
//
//Set database name:
$dbname = "Statistics";
//
//Get environment variables that
//we'll use for stats:
$ip = getenv("REMOTE_ADDR");
$host = getenv("HTTP_HOST");
$browser = getenv("HTTP_USER_AGENT");
$referer = getenv("HTTP_REFERER");
//
//Connect to MySQL - the @
//sign tells php to suppress
//mysql errors in favor of
//the "..or die.." clause
$cn = @mysql_connect("localhost", "user", "password") or die("Couldn't connect to SQL server");
//
//Select the database:
$db = @mysql_select_db($dbname, $cn) or die("Couldn't Access database: $dbname");
//
//Create our validation sql here -
//This query will allow us to log
//ONLY unique visitors
//and NOT duplicates or people
//that refresh the page 83 times
$sql = "SELECT ipaddress FROM stats WHERE ipaddress = \"$ip\"";
//
//Execute query:
$result = @mysql_query($sql, $cn) or die("Couldn't execute query");
//
//See if there were any matches
//by checking for an empty recordset
$num = mysql_numrows($result);
//
//If there were 0 results
//for that query then we're PRETTY
//sure that this is a unique
//visitor( We can't be POSITIVE
//because dial-up accounts will
//change ip's on us - at least
//if they refresh the page, we're not
//updating our hits counter every time)
if( $num == 0 ) {
//Create a uniqe stats entry:
$sql = "INSERT INTO stats(ipaddress, host, browser, referer) VALUES(\"$ip\", \"$host\", \"$browser\", \"$referer\")";
//
$result = @mysql_query($sql, $cn) or die("Couldn't update stats");
//
//Now update our hits count:
$sql = "UPDATE hits SET count = count + 1";
$result = @mysql_query($sql, $cn) or die("Couldn't update hits");
//
//You could even email yourself a notification
//that you've had a new visitor
mail("youremail@yourdomain.com", "You had a new visitor", "From $ip\nHost: $host", "FROM: stats@yourwebsite.com");
}
```

