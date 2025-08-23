# SonicSQL (ssql) – Complete Guide

**SonicSQL** is a lightweight, multi-mode SQL helper for **PHP** and **JavaScript**.  
It secures and simplifies database queries and gives you flexible return formats depending on your use case.

---

## ⚙️ Setup

1. Once you've purchased **SonicSQL**, upload `ssql.php` and `ssql.js` to your server, or move them to your working environment.  
2. Open `ssql.php` and enter your **mysqli connection details** at the top, then save and close.

---

## ⚡️ Connecting
You can connect automatically using the default connect in ssql.php or you can create a new connection as the first argument.

```php
$server = "localhost";
$username = "root";
$password = " ";
$database = "my_db";

$connect = new mysqli($server, $username, $password, $database);

ssql($connect,"SELECT * FROM users");
```
**Currently not supported in JS**

---

## 🔹 Working in PHP
```php
require "ssql.php";
```
## Return results
```php
$rows = ssql("SELECT * FROM users");

foreach($rows as $row){
    $name = $row['name'];
    echo "<p>$name</p>";
}
```

---

## 🔹 Working in JavaScript
```js
<script src="ssql.js"></script>
```
## Return results
```js
ssql("SELECT * FROM users").then(rows => {
    rows.forEach(row => {
        console.log(row.name);
    });
});
```

---

## 🎛 Modes Overview
When calling ssql, you can specify a mode as the last argument.
Default mode (no mode selected) returns all rows.
For example, using the "first" mode returns the first row.

PHP
```php
$result = ssql("SELECT * FROM users", [], "first");
print_r($result);
```

JS
```js
ssql("SELECT * FROM users", [], "first").then(result => {
    console.log(result);
});
```

---

## 🧩 Mode Reference
1. **Default / None** - Returns all rows as an array of associative arrays.
---
2. **first** - Returns the first row only.

PHP
```php
$row = ssql("SELECT * FROM users", [], "first");
echo $row['name'];
```

JS
```js
ssql("SELECT * FROM users", [], "first").then(row => {
    console.log(row.name);
});
```
---
3. **value** - Returns a single value if only one row/column selected.

PHP
```php
$name = ssql("SELECT name FROM users WHERE id=?", [1], "value");
```

JS
```js
ssql("SELECT name FROM users WHERE id=?", [1], "value").then(name => {
    console.log(name);
});
```
---
4. **json** - Returns results as JSON string.

PHP
```php
echo $json = ssql("SELECT * FROM users", [], "json");
```

JS
```js
ssql("SELECT * FROM users", [], "json").then(json => {
    console.log(json);
});
```
---
5. **random**

Returns selected rows in a random order.

---
6. **success**

Returns true if the query executed successfully.
Useful for INSERT, UPDATE, DELETE.

PHP
```php
$ok = ssql("UPDATE users SET active=? WHERE id=?", [1, 5], "success");
```
JS
```js
ssql("UPDATE users SET active=? WHERE id=?", [1, 5], "success").then(outcome => {
    console.log(outcome);
});
```
---
7. **sql**

Returns the SQL query string (for debugging).

---
8. **id**

Returns the last inserted ID (after INSERT).

---
9. **count**

Returns the number of rows.

---
10. **exists**

Returns true if any rows exist.

---
11. **column**

Returns a flat array of values from the first column.

---
12. **columns**

Returns a list of column names.

---
13. **flat**

If one column returned, returns a single value (or array of values).

---
14. **csv**

Returns the result as a CSV string (with headers).

---
15. **benchmark**

Returns query duration & row count

Useful for testing and optimising performance.

PHP
```php
$result = ssql("SELECT * FROM users WHERE active=?", [1], "benchmark");

echo "Time: " . $result['time'] . " seconds";
echo "Rows: " . $result['rows'];
```

JS
```js
ssql("SELECT * FROM users WHERE active=?", [1], "benchmark").then(result => {
    console.log("Time:", result.time, "seconds");
    console.log("Rows:", result.rows);
});
```
---
    
## 🔐 Parameters & Prepared Statements
Both PHP and JS support placeholders (?) with values array:

PHP
```php
$rows = ssql("SELECT * FROM users WHERE age > ? AND name LIKE ?", [18, "J%"]);
```
JS
```js
ssql("SELECT * FROM users WHERE age > ? AND name LIKE ?", [18, "J%"]).then(rows => {
    console.log(rows);
});
```
---
You can replace the square brackets with an array.

PHP
```php
$values = array(18, "J%");
$rows = ssql("SELECT * FROM users WHERE age > ? AND name LIKE ?", $values);
```
JS
```js
const values = [18, "J%"];
ssql("SELECT * FROM users WHERE age > ? AND name LIKE ?", values).then(rows => {
    console.log(rows);
});
```

---
## 🚀 Best Practices
• Use prepared statements (? placeholders) to prevent SQL injection.

• Use first, value, count, exists for efficiency when you don’t need the full dataset.

• Use json when returning data to APIs.

• Use csv for quick exports.

• In JS, always handle .then() because the results are asynchronous.

---

