# Unit 3: Array and Function
**Course:** Internet Technology II (CMP 273) · **Teaching hours:** 7

## Learning Objectives

After this unit, you will be able to:

- Explain the concept of an array and why arrays are important in PHP programs.
- Create and use indexed, associative and multi-dimensional arrays.
- Iterate over arrays using `for`, `foreach`, `while` and inspect them with `print_r()` and `var_dump()`.
- Explain what a function is and how functions support code reusability.
- Use PHP's built-in functions to work with strings, numbers, dates and times, and arrays.
- Write user-defined functions that accept arguments (by value and by reference) and return values.
- Explain variable scope in PHP: local, global, static and superglobal variables.

---

## 3.1 Working with Array

### What is an array?

An **array** is a single variable that stores **many values**. Each value in an array is called an **element**, and each element is identified by a **key** (also called an index).

Without arrays, storing the marks of 5 students needs 5 separate variables:

```php
<?php
$mark1 = 78;
$mark2 = 65;
$mark3 = 90;
$mark4 = 55;
$mark5 = 82;
```

With an array, one variable holds all of them:

```php
<?php
$marks = [78, 65, 90, 55, 82];
```

### Why arrays matter

- **Grouping related data.** A list of products, a list of students, the rows returned from a database query: all of these are naturally arrays.
- **Looping.** You can process every element with one loop instead of writing the same code again and again.
- **Form and request data.** PHP's superglobals `$_GET`, `$_POST`, `$_SESSION`, `$_COOKIE` and `$_FILES` are all arrays.
- **Database results.** `mysqli_fetch_assoc()` returns each row as an associative array (you will use this in the database unit).
- **Flexible size.** A PHP array grows and shrinks automatically. You do not declare its size in advance, unlike arrays in C.

### How PHP arrays work internally

In PHP, every array is actually an **ordered map**: a list of `key => value` pairs that remembers the order in which the pairs were added. Keys can be **integers** or **strings**. Values can be of **any type**, including other arrays.

```
  $marks = [78, 65, 90];

   key  |  value
  ------+-------
    0   |   78
    1   |   65
    2   |   90
```

PHP has three common "kinds" of arrays. They are all the same data type (`array`); the difference is only in how the keys are used.

| Kind | Keys | Example |
|------|------|---------|
| Indexed array | Integers, starting at 0 | `["Ram", "Sita", "Hari"]` |
| Associative array | Strings you choose | `["name" => "Ram", "age" => 20]` |
| Multi-dimensional array | Any; values are arrays | `[["Ram", 78], ["Sita", 85]]` |

### Creating arrays: two syntaxes

```php
<?php
// Short syntax (recommended, PHP 5.4+)
$colors = ["red", "green", "blue"];

// Long syntax using array() (older code and many textbooks)
$colors = array("red", "green", "blue");
```

Both create exactly the same array. These notes use the short `[]` syntax.

> **Teacher's Note:** Start with the "5 separate variables" example on the board and ask students how they would store marks for 60 students. The pain of writing `$mark1` to `$mark60` makes the need for arrays obvious. Compare with JavaScript arrays they already know from Internet Technology I: `let a = [1, 2, 3];` looks almost the same in PHP.

### 3.1.1 Indexed Array

An **indexed array** (also called a numeric array) uses **integer keys**. By default PHP assigns keys automatically starting from **0**.

#### Creating and accessing an indexed array

```php
<?php
// Example: Indexed array of cities
$cities = ["Kathmandu", "Pokhara", "Biratnagar", "Butwal"];

echo $cities[0] . "<br>";   // first element
echo $cities[2] . "<br>";   // third element
echo "Total cities: " . count($cities);
```

**Output:**
```
Kathmandu<br>Biratnagar<br>Total cities: 4
```

(In a browser, each `<br>` shows as a line break.)

#### Adding, changing and assigning keys manually

```php
<?php
// Example: Building an indexed array step by step
$fruits = [];            // empty array
$fruits[] = "Apple";     // [] appends at the next index (0)
$fruits[] = "Mango";     // index 1
$fruits[] = "Banana";    // index 2

$fruits[1] = "Orange";   // change the value at index 1

$fruits[10] = "Grapes";  // you may choose the key yourself
$fruits[] = "Litchi";    // next index is 11 (largest key + 1)

print_r($fruits);
```

**Output:**
```
Array
(
    [0] => Apple
    [1] => Orange
    [2] => Banana
    [10] => Grapes
    [11] => Litchi
)
```

Key points:

- `$array[] = value;` appends a value at the next free integer key.
- The next key is always **largest integer key so far + 1**, not "count of elements".
- Keys do not need to be continuous.

> **Common Mistake:** Accessing an index that does not exist.
> ```php
> $cities = ["Kathmandu", "Pokhara"];
> echo $cities[2];   // Warning: Undefined array key 2
> ```
> The last valid index is `count($cities) - 1`, which is `1` here. Check first with `isset($cities[2])` or use the null coalescing operator: `echo $cities[2] ?? "Not found";`

### 3.1.2 Associative Array

An **associative array** uses **named (string) keys** that you choose. It is useful when each value has a meaning, such as a name, an email or a price. It is similar to a JavaScript object or a Python dictionary.

Syntax: `key => value`

```php
<?php
// Example: Student record as an associative array
$student = [
    "name"    => "Sita Sharma",
    "roll"    => 15,
    "program" => "BCSIT",
    "city"    => "Pokhara",
];

echo "Name: " . $student["name"] . "\n";
echo "Roll: " . $student["roll"] . "\n";

$student["email"] = "sita@example.com";  // add a new key
$student["city"]  = "Lalitpur";          // update an existing key
unset($student["roll"]);                 // remove a key

print_r($student);
```

**Output:**
```
Name: Sita Sharma
Roll: 15
Array
(
    [name] => Sita Sharma
    [program] => BCSIT
    [city] => Lalitpur
    [email] => sita@example.com
)
```

#### Using array values inside strings

```php
<?php
// Example: Interpolating associative array values
$product = ["name" => "Momo", "price" => 150];

echo "Item: {$product['name']}, Price: Rs. {$product['price']}\n";
echo "Item: " . $product["name"] . "\n";
```

**Output:**
```
Item: Momo, Price: Rs. 150
Item: Momo
```

Inside a double-quoted string, wrap the array access in curly braces `{...}`. This is the clearest and safest form.

#### Indexed vs associative array

| Feature | Indexed Array | Associative Array |
|---------|---------------|-------------------|
| Key type | Integer (0, 1, 2, ...) | String (you choose the name) |
| Keys assigned | Automatically (or manually) | Manually, by the programmer |
| Access | `$a[0]` | `$a["name"]` |
| Best for | Ordered list of similar items | Record with named fields |
| Example | List of cities | One student's details |
| Typical loop | `for` or `foreach` | `foreach ($a as $key => $value)` |

> **Common Mistake:** Forgetting the quotes around a string key: `$student[name]`. In PHP 8 this is an **Error** ("Undefined constant name"). Always write `$student["name"]` or `$student['name']`.

> **Exam Tip:** "Differentiate between indexed and associative arrays with example" is a frequent 5-mark question. Draw the comparison table and give one short code example of each, with its output.

### 3.1.3 Array Iteration

**Iteration** means visiting each element of an array one by one. PHP gives several ways to do it.

#### a) `for` loop (indexed arrays only)

Use `for` when keys are continuous integers starting at 0.

```php
<?php
// Example: Print marks using a for loop
$marks = [78, 65, 90, 55, 82];
$n = count($marks);   // calculate once, not on every loop

for ($i = 0; $i < $n; $i++) {
    echo "Student " . ($i + 1) . ": " . $marks[$i] . "\n";
}
```

**Output:**
```
Student 1: 78
Student 2: 65
Student 3: 90
Student 4: 55
Student 5: 82
```

#### b) `foreach` loop (works for every array)

`foreach` is the most common and recommended way to loop over arrays in PHP. It has two forms.

**Form 1: value only**

```php
<?php
// Example: foreach with value only
$subjects = ["PHP", "Java", "DBMS", "Statistics"];

foreach ($subjects as $subject) {
    echo $subject . "\n";
}
```

**Output:**
```
PHP
Java
DBMS
Statistics
```

**Form 2: key => value**

```php
<?php
// Example: foreach with key => value on an associative array
$prices = ["Momo" => 150, "Chowmein" => 120, "Thukpa" => 180];

foreach ($prices as $item => $price) {
    echo "$item costs Rs. $price\n";
}
```

**Output:**
```
Momo costs Rs. 150
Chowmein costs Rs. 120
Thukpa costs Rs. 180
```

The same form works on indexed arrays too; the key is then the integer index.

**Modifying elements inside `foreach`**

By default `foreach` works on a **copy** of each value. To change the original array, use a reference (`&`):

```php
<?php
// Example: Add 5 grace marks to every student
$marks = [78, 65, 90];

foreach ($marks as &$m) {
    $m = $m + 5;
}
unset($m);   // important: break the reference after the loop

print_r($marks);
```

**Output:**
```
Array
(
    [0] => 83
    [1] => 70
    [2] => 95
)
```

> **Common Mistake:** Not calling `unset($m)` after a `foreach` by reference. `$m` still points to the last element, so a later `$m = 0;` would silently change `$marks[2]`. Always `unset()` the reference variable after the loop.

#### c) `while` loop with `count()`

```php
<?php
// Example: while loop over an indexed array
$cities = ["Kathmandu", "Pokhara", "Chitwan"];
$i = 0;

while ($i < count($cities)) {
    echo $cities[$i] . "\n";
    $i++;
}
```

**Output:**
```
Kathmandu
Pokhara
Chitwan
```

`do...while` can also be used, but remember it runs the body at least once, even for an empty array.

#### d) Displaying a whole array: `print_r()` and `var_dump()`

These two functions are mainly for **debugging**. They show the whole structure of an array.

```php
<?php
// Example: print_r vs var_dump
$info = ["name" => "Hari", "age" => 21, "passed" => true];

print_r($info);
var_dump($info);
```

**Output:**
```
Array
(
    [name] => Hari
    [age] => 21
    [passed] => 1
)
array(3) {
  ["name"]=>
  string(4) "Hari"
  ["age"]=>
  int(21)
  ["passed"]=>
  bool(true)
}
```

In a browser, wrap them in `<pre>` tags so that the line breaks and indentation are visible:

```php
<?php
echo "<pre>";
print_r($info);
echo "</pre>";
```

| Feature | `print_r()` | `var_dump()` |
|---------|-------------|--------------|
| Shows values | Yes | Yes |
| Shows data types | No | Yes (`int`, `string`, `bool`, ...) |
| Shows lengths | No | Yes (string length, element count) |
| `true` / `false` shown as | `1` / empty | `bool(true)` / `bool(false)` |
| Can return as a string | Yes: `print_r($a, true)` | No (prints directly) |
| Use for | Quick readable look | Exact debugging |

#### Which loop should I use?

| Loop | Works with | When to use |
|------|------------|-------------|
| `for` | Indexed arrays with keys 0..n-1 | You need the index number or want to skip/step |
| `foreach` | All arrays | Default choice; cleanest and safest |
| `while` | Indexed arrays | Loop condition is not just "until the end" |
| `print_r` / `var_dump` | All arrays | Debugging only, not for user-facing output |

> **Teacher's Note:** Show the same array printed with all four techniques in one file. Then change the indexed array to have keys `0, 1, 5`; the `for` loop now prints a warning while `foreach` still works. This demo convinces students why `foreach` is the default choice. (Time: about 15 minutes.)

### 3.1.4 Multi-dimensional Array

A **multi-dimensional array** is an array whose elements are themselves arrays. A **two-dimensional (2D)** array is like a table: rows and columns.

#### 2D indexed array

```php
<?php
// Example: 2D indexed array (rows and columns)
$matrix = [
    [1, 2, 3],
    [4, 5, 6],
];

echo $matrix[1][2] . "\n";   // row 1, column 2

foreach ($matrix as $row) {
    echo implode(" ", $row) . "\n";
}
```

**Output:**
```
6
1 2 3
4 5 6
```

#### Array of associative arrays (the most common real-world form)

This is exactly the shape of data you get from a database query: many rows, each row a record with named columns.

```
  $students
  +-----+--------------------------------------------------+
  |  0  | ["name"=>"Aarav", "php"=>78, "java"=>85, "db"=>72] |
  |  1  | ["name"=>"Sita",  "php"=>92, "java"=>88, "db"=>95] |
  |  2  | ["name"=>"Bikash","php"=>45, "java"=>38, "db"=>60] |
  +-----+--------------------------------------------------+
  $students[1]["java"]  -->  88
```

#### Example: Student marks table rendered as an HTML table

```php
<?php
// Example: Display student marks in an HTML table
$students = [
    ["name" => "Aarav Shrestha", "php" => 78, "java" => 85, "db" => 72],
    ["name" => "Sita Gurung",    "php" => 92, "java" => 88, "db" => 95],
    ["name" => "Bikash Thapa",   "php" => 45, "java" => 38, "db" => 60],
];
?>
<table border="1" cellpadding="6">
    <tr>
        <th>S.N.</th><th>Name</th><th>PHP</th><th>Java</th><th>DBMS</th>
        <th>Total</th><th>Result</th>
    </tr>
<?php foreach ($students as $i => $s):
    $total  = $s["php"] + $s["java"] + $s["db"];
    $passed = min($s["php"], $s["java"], $s["db"]) >= 40;
?>
    <tr>
        <td><?= $i + 1 ?></td>
        <td><?= htmlspecialchars($s["name"]) ?></td>
        <td><?= $s["php"] ?></td>
        <td><?= $s["java"] ?></td>
        <td><?= $s["db"] ?></td>
        <td><?= $total ?></td>
        <td><?= $passed ? "Pass" : "Fail" ?></td>
    </tr>
<?php endforeach; ?>
</table>
```

**Output (as seen in the browser):**

| S.N. | Name | PHP | Java | DBMS | Total | Result |
|------|------|-----|------|------|-------|--------|
| 1 | Aarav Shrestha | 78 | 85 | 72 | 235 | Pass |
| 2 | Sita Gurung | 92 | 88 | 95 | 275 | Pass |
| 3 | Bikash Thapa | 45 | 38 | 60 | 143 | Fail |

Things to notice:

- `foreach ($students as $i => $s)` gives the row number `$i` and the row itself `$s`.
- `$s["php"]` reads one column of the current row.
- The **alternative syntax** `foreach (...):` ... `endforeach;` and the short echo tag `<?= ... ?>` keep HTML templates readable.
- `htmlspecialchars()` is used on text that could come from users (names). This prevents XSS (Cross-Site Scripting). Numbers calculated by our own code are safe.

#### Nested loops over a 2D array

```php
<?php
// Example: Nested foreach to print every subject mark
$results = [
    "Ram"  => ["PHP" => 80, "Java" => 70],
    "Gita" => ["PHP" => 90, "Java" => 85],
];

foreach ($results as $name => $subjects) {
    echo "$name:\n";
    foreach ($subjects as $subject => $mark) {
        echo "  $subject = $mark\n";
    }
}
```

**Output:**
```
Ram:
  PHP = 80
  Java = 70
Gita:
  PHP = 90
  Java = 85
```

A **three-dimensional** array simply adds one more level, for example `$college["BCSIT"]["Sem3"][0]["name"]`. Arrays deeper than three levels are hard to read; use them rarely.

> **Exam Tip:** "Write a PHP program to store name and marks of N students in a multi-dimensional array and display them in a table with total and result" is a very common long question. Practice writing it fully from memory, including the `<table>` tags.

---

## 3.2 PHP's Built in Function

### What is a function?

A **function** is a named block of code that performs a specific task. You write (or PHP provides) it once and **call** it as many times as needed.

```
   input (arguments)  --->  [ function ]  --->  output (return value)
       "hello"              strtoupper()            "HELLO"
```

**Why functions matter (code reusability):**

- **Reuse:** write once, use many times, in many pages.
- **Readability:** `calculateGrade($marks)` explains itself.
- **Maintenance:** fix a bug in one place and it is fixed everywhere.
- **Testing:** small functions are easy to test separately.

PHP has two kinds of functions:

| Built-in functions | User-defined functions |
|--------------------|------------------------|
| Provided by PHP itself (thousands of them) | Written by the programmer |
| Ready to use, no definition needed | Must be defined with `function` keyword |
| Examples: `strlen()`, `count()`, `date()` | Examples: `calculateTotal()`, `getGrade()` |

Function names in PHP are **case-insensitive** (`STRLEN()` works), but always write them in lowercase as documented. The official manual at php.net lists every built-in function with examples; teach students to look things up there.

> **Teacher's Note:** You cannot cover every built-in function. Teach the groups below with one live example each, and give students the habit of checking the PHP manual for argument order. A good classroom game: write an input and an output on the board (e.g. "pokhara" -> "Pokhara") and ask which function does it.

### 3.2.1 String Function

A **string** is a sequence of characters. PHP provides many functions to measure, search, change and format strings.

#### Length, words and reverse

```php
<?php
// Example: strlen, str_word_count, strrev
$text = "Hello Pokhara";

echo strlen($text) . "\n";          // number of characters (bytes)
echo str_word_count($text) . "\n";  // number of words
echo strrev($text) . "\n";          // reverse the string
```

**Output:**
```
13
2
arahkoP olleH
```

Note: `strlen()` counts **bytes**. For Nepali (Devanagari) or other UTF-8 text, use `mb_strlen($text)` to count characters correctly.

#### Searching: `strpos()` and `str_contains()`

`strpos($haystack, $needle)` returns the **position** (starting at 0) of the first match, or `false` if not found.

```php
<?php
// Example: Finding text inside a string
$email = "ram@example.com";

$pos = strpos($email, "@");
echo "@ found at position: $pos\n";

if (strpos($email, "Ram") === false) {
    echo "'Ram' not found (search is case-sensitive)\n";
}

// PHP 8: str_contains returns true/false directly
if (str_contains($email, "example")) {
    echo "Email is from example.com\n";
}
var_dump(str_starts_with($email, "ram"));   // PHP 8
var_dump(str_ends_with($email, ".np"));     // PHP 8
```

**Output:**
```
@ found at position: 3
'Ram' not found (search is case-sensitive)
Email is from example.com
bool(true)
bool(false)
```

> **Common Mistake:** Checking `strpos()` with `==` or as a plain condition.
> ```php
> if (strpos("PHP is fun", "PHP")) { ... }   // WRONG: returns 0, which is falsy
> ```
> `strpos()` returns `0` when the match is at the start, and `0` is treated as false. Always compare with `=== false` / `!== false`, or in PHP 8 simply use `str_contains()`.

#### Replacing and extracting: `str_replace()` and `substr()`

```php
<?php
// Example: str_replace and substr
$msg = "I live in Kathmandu. Kathmandu is busy.";
echo str_replace("Kathmandu", "Pokhara", $msg) . "\n";

$str = "Internet Technology";
echo substr($str, 0, 8) . "\n";    // from index 0, 8 characters
echo substr($str, 9) . "\n";       // from index 9 to the end
echo substr($str, -4) . "\n";      // last 4 characters
```

**Output:**
```
I live in Pokhara. Pokhara is busy.
Internet
Technology
logy
```

`str_replace(search, replace, subject)` replaces **all** occurrences. `str_ireplace()` is the case-insensitive version.

#### Changing case

```php
<?php
// Example: Case conversion functions
$name = "ram bahadur THAPA";

echo strtoupper($name) . "\n";            // all uppercase
echo strtolower($name) . "\n";            // all lowercase
echo ucfirst("pokhara university") . "\n"; // first letter of string
echo ucwords(strtolower($name)) . "\n";    // first letter of each word
```

**Output:**
```
RAM BAHADUR THAPA
ram bahadur thapa
Pokhara university
Ram Bahadur Thapa
```

#### Trimming whitespace

`trim()` removes spaces, tabs and newlines from **both ends**. `ltrim()` removes from the left only, `rtrim()` from the right only. Always trim form input before validating it.

```php
<?php
// Example: trim, ltrim, rtrim
$input = "   sita@example.com   ";

echo "[" . trim($input) . "]\n";
echo "[" . ltrim($input) . "]\n";
echo "[" . rtrim($input) . "]\n";
echo "[" . trim("--BCSIT--", "-") . "]\n";  // trim given characters
```

**Output:**
```
[sita@example.com]
[sita@example.com   ]
[   sita@example.com]
[BCSIT]
```

#### Splitting and joining: `explode()` and `implode()`

- `explode(separator, string)` splits a string into an **array**.
- `implode(separator, array)` joins an array into a **string**. (`join()` is an alias.)

```php
<?php
// Example: explode and implode
$csv = "PHP,Java,DBMS,Statistics";
$subjects = explode(",", $csv);
print_r($subjects);

echo implode(" | ", $subjects) . "\n";

$fullName = "Hari Prasad Adhikari";
$parts = explode(" ", $fullName);
echo "Last name: " . end($parts) . "\n";
```

**Output:**
```
Array
(
    [0] => PHP
    [1] => Java
    [2] => DBMS
    [3] => Statistics
)
PHP | Java | DBMS | Statistics
Last name: Adhikari
```

#### Formatting: `sprintf()`, `printf()` and `number_format()`

`sprintf()` returns a formatted string using **placeholders**; `printf()` prints it directly.

| Placeholder | Meaning | Example |
|-------------|---------|---------|
| `%s` | String | `sprintf("%s", "Ram")` -> `Ram` |
| `%d` | Integer | `sprintf("%d", 45.9)` -> `45` |
| `%f` / `%.2f` | Float (2 decimals) | `sprintf("%.2f", 3.14159)` -> `3.14` |
| `%05d` | Pad with zeros to width 5 | `sprintf("%05d", 42)` -> `00042` |
| `%%` | A literal `%` sign | `sprintf("%d%%", 80)` -> `80%` |

`number_format(number, decimals)` adds thousands separators.

```php
<?php
// Example: Formatting a receipt line
$item  = "Laptop";
$price = 125000.5;
$pct   = 78.456;

echo sprintf("Item: %s, Price: Rs. %.2f", $item, $price) . "\n";
printf("Percentage: %.1f%%\n", $pct);
printf("Invoice No: INV-%05d\n", 27);

echo number_format($price) . "\n";        // no decimals
echo number_format($price, 2) . "\n";     // 2 decimals
```

**Output:**
```
Item: Laptop, Price: Rs. 125000.50
Percentage: 78.5%
Invoice No: INV-00027
125,001
125,000.50
```

Note that `number_format($price)` with no decimals **rounds** 125000.5 to 125,001.

#### Other useful string functions

| Function | Purpose | Example -> Result |
|----------|---------|-------------------|
| `strcmp($a, $b)` | Compare (case-sensitive); 0 if equal | `strcmp("a", "a")` -> `0` |
| `strcasecmp($a, $b)` | Compare ignoring case | `strcasecmp("PHP", "php")` -> `0` |
| `str_repeat($s, $n)` | Repeat a string | `str_repeat("-", 5)` -> `-----` |
| `str_pad($s, $len, $pad)` | Pad to a length | `str_pad("7", 3, "0", STR_PAD_LEFT)` -> `007` |
| `nl2br($s)` | Turn newlines into `<br>` | useful for textarea output |
| `htmlspecialchars($s)` | Escape `< > & " '` for safe HTML output | `<b>` -> `&lt;b&gt;` |
| `strip_tags($s)` | Remove HTML tags | `strip_tags("<b>Hi</b>")` -> `Hi` |

#### String functions quick reference

| Function | What it does |
|----------|--------------|
| `strlen($s)` | Length in bytes |
| `str_word_count($s)` | Number of words |
| `strrev($s)` | Reverse |
| `strpos($s, $find)` | Position of first match, or `false` |
| `str_replace($find, $rep, $s)` | Replace all matches |
| `substr($s, $start, $len)` | Part of a string |
| `strtoupper` / `strtolower` | Change case |
| `ucfirst` / `ucwords` | Capitalise first letter / each word |
| `trim` / `ltrim` / `rtrim` | Remove whitespace |
| `explode` / `implode` | String to array / array to string |
| `sprintf` / `number_format` | Format numbers and text |
| `str_contains` (PHP 8) | `true` if substring exists |

> **Exam Tip:** Questions like "Explain any five string functions with example" are common. For each function write: the syntax, one line of purpose, a one-line example and its output. A table like the one above plus a short program scores well.

### 3.2.2 Math Function

PHP has built-in functions for common mathematical work. They work with both integers and floats.

```php
<?php
// Example: Basic math functions
echo abs(-15) . "\n";          // absolute value
echo round(4.567) . "\n";      // nearest integer
echo round(4.567, 2) . "\n";   // 2 decimal places
echo round(2.5) . "\n";        // halves round away from zero
echo ceil(4.1) . "\n";         // round UP
echo floor(4.9) . "\n";        // round DOWN
echo sqrt(81) . "\n";          // square root
echo pow(2, 10) . "\n";        // 2 to the power 10
echo 2 ** 10 . "\n";           // same, using the ** operator
```

**Output:**
```
15
5
4.57
3
5
4
9
1024
1024
```

#### `round()`, `ceil()` and `floor()` compared

| Value | `round()` | `ceil()` | `floor()` |
|-------|-----------|----------|-----------|
| 4.3 | 4 | 5 | 4 |
| 4.5 | 5 | 5 | 4 |
| 4.7 | 5 | 5 | 4 |
| -4.3 | -4 | -4 | -5 |
| -4.7 | -5 | -4 | -5 |

Note: `round()`, `ceil()` and `floor()` return a **float** (e.g. `float(5)`), even though `echo` shows `5`.

#### Max, min, random numbers and division

```php
<?php
// Example: max, min, intdiv, fmod, pi, random numbers
$marks = [78, 65, 90, 55, 82];

echo "Highest: " . max($marks) . "\n";   // accepts an array
echo "Lowest: " . min(12, 7, 30) . "\n"; // or separate values

echo intdiv(17, 5) . "\n";      // integer division: 3
echo 17 % 5 . "\n";             // integer remainder: 2
echo fmod(10.5, 3) . "\n";      // float remainder: 1.5
echo round(pi(), 4) . "\n";     // value of pi

$dice = rand(1, 6);             // random integer 1..6 (not secure)
$otp  = random_int(100000, 999999);  // cryptographically secure
echo "Dice: $dice, OTP: $otp\n";
```

**Output (the last line changes on every run):**
```
Highest: 90
Lowest: 7
3
2
1.5
3.1416
Dice: 4, OTP: 583920
```

| Function | Purpose |
|----------|---------|
| `abs($x)` | Absolute (positive) value |
| `round($x, $p)` | Round to nearest, `$p` decimal places |
| `ceil($x)` | Round up to next integer |
| `floor($x)` | Round down to previous integer |
| `sqrt($x)` | Square root |
| `pow($x, $y)` | `$x` raised to `$y` (same as `$x ** $y`) |
| `max(...)` / `min(...)` | Largest / smallest of values or of an array |
| `rand($min, $max)` | Random integer (fast, **not** secure) |
| `random_int($min, $max)` | Secure random integer (OTPs, tokens) |
| `intdiv($a, $b)` | Integer part of division |
| `fmod($a, $b)` | Remainder of float division |
| `pi()` / `M_PI` | 3.14159... |

> **Common Mistake:** Using `rand()` or `mt_rand()` to generate OTP codes, password-reset tokens or session keys. These are predictable. Use `random_int()` for numbers and `bin2hex(random_bytes(16))` for tokens.

### 3.2.3 Date and Time Function

Websites constantly need dates: order dates, published dates, exam schedules, "member since", age calculation. PHP stores time as a **Unix timestamp**: the number of seconds since **1 January 1970 00:00:00 UTC** (the "Unix epoch").

```
  1970-01-01 00:00:00 UTC  ------------------------------>  now
            0                    seconds counted            time()
```

#### Setting the timezone

PHP's default timezone may be UTC, which is 5 hours 45 minutes behind Nepal. Always set the timezone at the top of your script (or in `php.ini` as `date.timezone = Asia/Kathmandu`):

```php
<?php
date_default_timezone_set('Asia/Kathmandu');
echo date_default_timezone_get();   // Asia/Kathmandu
```

> **Common Mistake:** Forgetting the timezone, then wondering why the time shown is 5:45 hours off. Set `Asia/Kathmandu` once at the start (for example in a shared `config.php` file that every page includes).

#### `time()` and `date()`

- `time()` returns the current Unix timestamp (an integer).
- `date(format, timestamp)` turns a timestamp into a readable string. If the timestamp is omitted, the current time is used.

```php
<?php
// Example: Current date and time
date_default_timezone_set('Asia/Kathmandu');

echo time() . "\n";
echo date("Y-m-d") . "\n";
echo date("l, d F Y") . "\n";
echo date("h:i:s A") . "\n";
```

**Output (depends on when you run it):**
```
1795321800
2026-11-22
Sunday, 22 November 2026
10:15:00 AM
```

#### Date format characters

| Character | Meaning | Example |
|-----------|---------|---------|
| `d` | Day of month, 2 digits | `01` to `31` |
| `j` | Day of month, no leading zero | `1` to `31` |
| `D` | Short day name | `Mon` |
| `l` (lowercase L) | Full day name | `Monday` |
| `N` | Day of week number (1 = Monday, 7 = Sunday) | `1` |
| `m` | Month, 2 digits | `01` to `12` |
| `n` | Month, no leading zero | `1` to `12` |
| `M` | Short month name | `Jan` |
| `F` | Full month name | `January` |
| `Y` | Year, 4 digits | `2026` |
| `y` | Year, 2 digits | `26` |
| `H` | Hour, 24-hour format | `00` to `23` |
| `h` | Hour, 12-hour format | `01` to `12` |
| `i` | Minutes | `00` to `59` |
| `s` | Seconds | `00` to `59` |
| `A` / `a` | AM/PM, uppercase / lowercase | `PM` / `pm` |
| `t` | Number of days in the month | `28` to `31` |
| `L` | Leap year? (1 or 0) | `0` |

Common combinations: `"Y-m-d"` (database format, `2026-11-22`), `"d/m/Y"` (`22/11/2026`), `"D, d M Y H:i"` (`Sun, 22 Nov 2026 10:15`).

#### `mktime()`: create a timestamp for a specific date

Syntax: `mktime(hour, minute, second, month, day, year)`. Note the order: **month before day**.

```php
<?php
// Example: Timestamp for a specific date
date_default_timezone_set('Asia/Kathmandu');

$exam = mktime(10, 30, 0, 12, 15, 2026);   // 15 Dec 2026, 10:30 AM
echo "Exam: " . date("l, d M Y h:i A", $exam) . "\n";

$days = date("t", mktime(0, 0, 0, 2, 1, 2028));
echo "Days in Feb 2028: $days\n";
```

**Output:**
```
Exam: Tuesday, 15 Dec 2026 10:30 AM
Days in Feb 2028: 29
```

#### `strtotime()`: convert English text to a timestamp

`strtotime()` understands many English date phrases and returns a timestamp (or `false` if it cannot understand the text). An optional second argument gives the base time.

```php
<?php
// Example: strtotime with absolute and relative dates
date_default_timezone_set('Asia/Kathmandu');

$base = strtotime("2026-11-22");   // fixed base date for a predictable output

echo date("Y-m-d", strtotime("+1 day", $base)) . "\n";
echo date("Y-m-d", strtotime("+1 week", $base)) . "\n";
echo date("Y-m-d", strtotime("next Monday", $base)) . "\n";
echo date("Y-m-d", strtotime("last day of this month", $base)) . "\n";
echo date("d M Y", strtotime("15 August 2027")) . "\n";
```

**Output:**
```
2026-11-23
2026-11-29
2026-11-23
2026-11-30
15 Aug 2027
```

In real code you normally omit the second argument, so the phrase is relative to "now": `strtotime("+7 days")`.

#### `checkdate()`: validate a date

`checkdate(month, day, year)` returns `true` if the date is valid. It is useful for checking dates entered in forms.

```php
<?php
// Example: Validating dates
var_dump(checkdate(2, 29, 2028));  // 2028 is a leap year
var_dump(checkdate(2, 29, 2027));  // 2027 is not
var_dump(checkdate(4, 31, 2026));  // April has 30 days
```

**Output:**
```
bool(true)
bool(false)
bool(false)
```

#### The `DateTime` class and `diff()`: calculating age

For calculations between two dates, the object-oriented `DateTime` class is more reliable than manual arithmetic with seconds (it handles leap years and month lengths correctly).

```php
<?php
// Example: Age calculation using DateTime and diff()
date_default_timezone_set('Asia/Kathmandu');

$dob   = new DateTime("2005-03-14");
$today = new DateTime("2026-11-22");   // use new DateTime() for the real today

$age = $dob->diff($today);             // returns a DateInterval object

echo "Age: {$age->y} years, {$age->m} months, {$age->d} days\n";
echo "Total days lived: {$age->days}\n";

echo $dob->format("l, d F Y") . "\n";  // format works like date()

$dob->modify("+18 years");
echo "Eligible to vote from: " . $dob->format("Y-m-d") . "\n";
```

**Output:**
```
Age: 21 years, 8 months, 8 days
Total days lived: 7923
Monday, 14 March 2005
Eligible to vote from: 2023-03-14
```

`DateInterval` properties: `y` (years), `m` (months), `d` (days), `h`, `i`, `s`, and `days` (total number of days).

> **Teacher's Note:** Ask each student to calculate their own age in years, months and days with `DateTime::diff()`. Then ask them to calculate "days left until the semester-end exam". Students enjoy personal examples, and it makes `diff()` memorable. Mention that Bikram Sambat (BS) dates are not built into PHP; converting BS to AD needs a lookup table or a library.

> **Exam Tip:** Learn at least 10 format characters (`d, D, l, m, M, F, Y, y, H, h, i, s, A`). "Write a PHP script to display the current date in the format: Sunday, 22 November 2026" is a typical short question. Answer: `echo date("l, d F Y");`

### 3.2.4 Array Function

PHP has more than 70 array functions. These are the ones you will use most.

#### Counting and adding/removing elements

| Function | Action | Position |
|----------|--------|----------|
| `array_push($a, $v1, ...)` | Add element(s) | End |
| `array_pop($a)` | Remove and return last element | End |
| `array_unshift($a, $v1, ...)` | Add element(s) | Beginning |
| `array_shift($a)` | Remove and return first element | Beginning |

```
             array_unshift            array_push
                  |                       |
                  v                       v
               [ "A",  "B",  "C",  "D" ]
                  ^                       ^
                  |                       |
             array_shift              array_pop
```

```php
<?php
// Example: count, push, pop, shift, unshift
$queue = ["Ram", "Sita"];

array_push($queue, "Hari", "Gita");   // add to the end
array_unshift($queue, "Anil");        // add to the beginning
echo implode(", ", $queue) . "\n";

$last  = array_pop($queue);           // remove from the end
$first = array_shift($queue);         // remove from the beginning
echo "Removed: $first and $last\n";
echo implode(", ", $queue) . "\n";
echo "Count: " . count($queue) . "\n";
```

**Output:**
```
Anil, Ram, Sita, Hari, Gita
Removed: Anil and Gita
Ram, Sita, Hari
Count: 3
```

`count()` (alias `sizeof()`) returns the number of elements. `count($a, COUNT_RECURSIVE)` also counts elements inside nested arrays.

#### Searching: `in_array()` and `array_search()`

```php
<?php
// Example: in_array and array_search
$subjects = ["PHP", "Java", "DBMS"];

if (in_array("Java", $subjects)) {
    echo "Java is offered\n";
}

$pos = array_search("DBMS", $subjects);   // returns the key, or false
echo "DBMS is at index $pos\n";

var_dump(in_array("10", [10, 20]));        // loose comparison
var_dump(in_array("10", [10, 20], true));  // strict: type must match
```

**Output:**
```
Java is offered
DBMS is at index 2
bool(true)
bool(false)
```

Like `strpos()`, `array_search()` can return `0`, so check it with `!== false`.

#### Keys and values: `array_keys()` and `array_values()`

```php
<?php
// Example: array_keys and array_values
$prices = ["Momo" => 150, "Chowmein" => 120, "Thukpa" => 180];

print_r(array_keys($prices));
print_r(array_values($prices));
var_dump(array_key_exists("Momo", $prices));
```

**Output:**
```
Array
(
    [0] => Momo
    [1] => Chowmein
    [2] => Thukpa
)
Array
(
    [0] => 150
    [1] => 120
    [2] => 180
)
bool(true)
```

`array_values()` is also handy to **re-index** an array after removing elements with `unset()`.

#### Merging: `array_merge()`

```php
<?php
// Example: array_merge with indexed and associative arrays
$sem1 = ["C", "Maths"];
$sem2 = ["PHP", "Java"];
print_r(array_merge($sem1, $sem2));

$default = ["theme" => "light", "lang" => "en"];
$user    = ["lang" => "ne"];
print_r(array_merge($default, $user));   // later string keys overwrite
```

**Output:**
```
Array
(
    [0] => C
    [1] => Maths
    [2] => PHP
    [3] => Java
)
Array
(
    [theme] => light
    [lang] => ne
)
```

With integer keys, `array_merge()` renumbers the elements. With string keys, the later array's value wins.

#### Extracting and cutting: `array_slice()` and `array_splice()`

| | `array_slice()` | `array_splice()` |
|---|---|---|
| Changes the original array? | No | **Yes** |
| Returns | The extracted part | The removed part |
| Can insert new elements? | No | Yes |

```php
<?php
// Example: array_slice vs array_splice
$days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

$work = array_slice($days, 1, 5);      // from index 1, take 5
echo implode(" ", $work) . "\n";
echo count($days) . "\n";              // original unchanged: 7

$nums = [10, 20, 30, 40, 50];
$removed = array_splice($nums, 1, 2, ["X", "Y", "Z"]);
echo "Removed: " . implode(" ", $removed) . "\n";
echo "Now: " . implode(" ", $nums) . "\n";
```

**Output:**
```
Mon Tue Wed Thu Fri
7
Removed: 20 30
Now: 10 X Y Z 40 50
```

#### Sorting functions

All sorting functions **change the original array** and return `true`. Do not write `$sorted = sort($a);`.

| Function | Sorts by | Order | Keeps key-value association? | Use for |
|----------|----------|-------|------------------------------|---------|
| `sort()` | Value | Ascending | No (re-indexes 0, 1, 2...) | Indexed arrays |
| `rsort()` | Value | Descending | No | Indexed arrays |
| `asort()` | Value | Ascending | Yes | Associative arrays |
| `arsort()` | Value | Descending | Yes | Associative arrays (e.g. rank list) |
| `ksort()` | Key | Ascending | Yes | Associative arrays (alphabetical keys) |
| `krsort()` | Key | Descending | Yes | Associative arrays |

Memory aid: **a** = keep **a**ssociation, **k** = sort by **k**ey, **r** = **r**everse.

```php
<?php
// Example: Comparing sort functions
$nums = [42, 7, 19, 3];
sort($nums);
echo "sort:   " . implode(", ", $nums) . "\n";
rsort($nums);
echo "rsort:  " . implode(", ", $nums) . "\n";

$marks = ["Ram" => 78, "Sita" => 92, "Hari" => 65];

asort($marks);
echo "asort:  " . json_encode($marks) . "\n";
arsort($marks);
echo "arsort: " . json_encode($marks) . "\n";
ksort($marks);
echo "ksort:  " . json_encode($marks) . "\n";
krsort($marks);
echo "krsort: " . json_encode($marks) . "\n";

sort($marks);   // names are lost!
echo "sort on assoc: " . json_encode($marks) . "\n";
```

**Output:**
```
sort:   3, 7, 19, 42
rsort:  42, 19, 7, 3
asort:  {"Hari":65,"Ram":78,"Sita":92}
arsort: {"Sita":92,"Ram":78,"Hari":65}
ksort:  {"Hari":65,"Ram":78,"Sita":92}
krsort: {"Sita":92,"Ram":78,"Hari":65}
sort on assoc: [65,78,92]
```

(`json_encode()` is used here only to show key-value pairs on one line.)

> **Common Mistake:** Using `sort()` on an associative array. It throws away the string keys, so you lose which mark belongs to which student. Use `asort()` / `arsort()` to sort by value while keeping keys.

For custom sorting (for example, sorting rows of a 2D array by a column) PHP provides `usort($array, $callback)`. Example: `usort($students, fn($a, $b) => $b["total"] <=> $a["total"]);` sorts students by total, highest first. The `<=>` (spaceship) operator returns -1, 0 or 1.

#### Calculating and cleaning: `array_sum()`, `array_unique()`, `array_reverse()`

```php
<?php
// Example: array_sum, array_product, array_unique, array_reverse
$marks = [78, 65, 90, 55, 82];
$total = array_sum($marks);
$avg   = $total / count($marks);
echo "Total: $total, Average: $avg\n";
echo "Product of [2,3,4]: " . array_product([2, 3, 4]) . "\n";

$cities = ["Pokhara", "Kathmandu", "Pokhara", "Butwal", "Kathmandu"];
$unique = array_unique($cities);
print_r($unique);                        // keys of first occurrences kept
echo implode(", ", array_values($unique)) . "\n";

echo implode(", ", array_reverse([1, 2, 3, 4])) . "\n";
```

**Output:**
```
Total: 370, Average: 74
Product of [2,3,4]: 24
Array
(
    [0] => Pokhara
    [1] => Kathmandu
    [3] => Butwal
)
Pokhara, Kathmandu, Butwal
4, 3, 2, 1
```

#### Transforming: `array_map()` and `array_filter()`

- `array_map(callback, array)` applies a function to **every** element and returns a new array of results.
- `array_filter(array, callback)` keeps only the elements for which the callback returns `true`. Keys are preserved.

```php
<?php
// Example: array_map and array_filter
$prices = [100, 250, 400];

// add 13% VAT to every price
$withVat = array_map(fn($p) => $p * 1.13, $prices);
print_r($withVat);

$names = array_map('strtoupper', ["ram", "sita"]);   // built-in name as callback
echo implode(", ", $names) . "\n";

$marks = [78, 35, 90, 25, 60];
$passed = array_filter($marks, fn($m) => $m >= 40);
print_r($passed);
```

**Output:**
```
Array
(
    [0] => 113
    [1] => 282.5
    [2] => 452
)
RAM, SITA
Array
(
    [0] => 78
    [2] => 90
    [4] => 60
)
```

`fn($p) => $p * 1.13` is an **arrow function** (a short anonymous function). They are explained in section 3.3.2.

#### Creating arrays: `range()`

```php
<?php
// Example: range
echo implode(" ", range(1, 10)) . "\n";
echo implode(" ", range(0, 50, 10)) . "\n";   // step of 10
echo implode(" ", range('a', 'e')) . "\n";
echo implode(" ", range(5, 1)) . "\n";        // counting down
```

**Output:**
```
1 2 3 4 5 6 7 8 9 10
0 10 20 30 40 50
a b c d e
5 4 3 2 1
```

#### `compact()` and `extract()` (briefly)

- `compact('var1', 'var2')` builds an associative array from **variable names**.
- `extract($array)` does the reverse: creates variables from an array's keys.

```php
<?php
// Example: compact and extract
$name = "Gita";
$age  = 20;
$student = compact('name', 'age');
print_r($student);

$book = ["title" => "PHP and MySQL", "author" => "Larry Ullman"];
extract($book);
echo "$title by $author\n";
```

**Output:**
```
Array
(
    [name] => Gita
    [age] => 20
)
PHP and MySQL by Larry Ullman
```

> **Common Mistake:** Never call `extract($_POST)` or `extract($_GET)`. A user could send a field named `isAdmin` and overwrite your own `$isAdmin` variable. Read form fields one by one with `filter_input()` instead.

#### Array functions quick reference

| Function | Purpose |
|----------|---------|
| `count($a)` | Number of elements |
| `array_push` / `array_pop` | Add / remove at end |
| `array_unshift` / `array_shift` | Add / remove at beginning |
| `in_array($v, $a)` | Does the value exist? (true/false) |
| `array_search($v, $a)` | Key of the value, or `false` |
| `array_keys` / `array_values` | All keys / all values |
| `array_key_exists($k, $a)` | Does the key exist? |
| `array_merge($a, $b)` | Join arrays |
| `array_slice($a, $off, $len)` | Copy a part (original unchanged) |
| `array_splice($a, $off, $len, $rep)` | Remove/replace a part (original changed) |
| `sort, rsort, asort, arsort, ksort, krsort` | Sort (see table above) |
| `array_sum` / `array_product` | Sum / product of values |
| `array_unique` | Remove duplicate values |
| `array_reverse` | Reverse the order |
| `array_map` | Apply a function to every element |
| `array_filter` | Keep elements matching a condition |
| `range($start, $end, $step)` | Create an array of a sequence |
| `compact` / `extract` | Variables to array / array to variables |

> **Exam Tip:** "Explain sort(), asort() and ksort() with example" or "Differentiate between array_slice() and array_splice()" are typical short questions. Use a small associative array of names and marks so the difference in keys is visible in your output.

---

## 3.3 User Defined Function

A **user-defined function** is a function that you write yourself to perform a task that PHP does not provide directly, or to group several steps under one name.

### Syntax

```php
function functionName($param1, $param2)
{
    // statements
    return $result;   // optional
}
```

Rules for function names:

- Start with a letter or underscore, followed by letters, digits or underscores.
- Names are **case-insensitive**, but call them exactly as defined for readability.
- You cannot define two functions with the same name (no overloading in PHP).
- Use descriptive verb-based names: `calculateTotal()`, `getGrade()`, `isValidEmail()`.

#### A first function

```php
<?php
// Example: Defining and calling a simple function
function greet()
{
    echo "Namaste! Welcome to Internet Technology II.\n";
}

greet();   // call 1
greet();   // call 2 - code reused
```

**Output:**
```
Namaste! Welcome to Internet Technology II.
Namaste! Welcome to Internet Technology II.
```

A function defined at the top level of a file can be called even **before** its definition appears in the file, because PHP reads the whole file first. In larger projects, functions are usually kept in a separate file (for example `functions.php`) and loaded with `require_once`.

#### Parameters vs arguments

- A **parameter** is the variable listed in the function definition: `function square($n)`.
- An **argument** is the actual value passed when calling: `square(5)`.

### 3.3.1 Passing arguments and Return

#### Returning a value

`return` sends a value back to the caller and **immediately ends** the function. A function without `return` returns `null`.

```php
<?php
// Example: Function that returns a grade
function getGrade($marks)
{
    if ($marks >= 90) return "A+";
    if ($marks >= 80) return "A";
    if ($marks >= 70) return "B+";
    if ($marks >= 60) return "B";
    if ($marks >= 40) return "C";
    return "F";
}

$list = ["Aarav" => 85, "Sita" => 93, "Bikash" => 38];
foreach ($list as $name => $m) {
    echo "$name: $m => " . getGrade($m) . "\n";
}
```

**Output:**
```
Aarav: 85 => A
Sita: 93 => A+
Bikash: 38 => F
```

> **Common Mistake:** Using `echo` inside a function when the caller needs the value. A function that `echo`es cannot be used in calculations or stored in a variable. Prefer **return** the value and let the caller decide whether to print it.

#### Passing by value vs passing by reference

**By value (default):** the function receives a **copy**. Changes inside the function do not affect the original variable.

**By reference (`&`):** the function receives the **original variable** itself. Changes inside the function affect the caller's variable.

```php
<?php
// Example: Pass by value vs pass by reference
function addBonusByValue($salary)
{
    $salary += 5000;
}

function addBonusByReference(&$salary)
{
    $salary += 5000;
}

$a = 30000;
addBonusByValue($a);
echo "After by value: $a\n";

$b = 30000;
addBonusByReference($b);
echo "After by reference: $b\n";
```

**Output:**
```
After by value: 30000
After by reference: 35000
```

```
  By value:                         By reference:
  $a [30000] --copy--> $salary       $b [30000] <---- $salary (same box)
                       [35000]            [35000]
  $a still 30000                     $b becomes 35000
```

| Feature | Pass by Value | Pass by Reference |
|---------|---------------|-------------------|
| Syntax | `function f($x)` | `function f(&$x)` |
| What is passed | A copy of the value | The variable itself (an alias) |
| Original changed? | No | Yes |
| Argument can be a literal? | Yes, `f(10)` | No, must be a variable |
| Safety | Safer, no side effects | Use only when you need to modify |
| Built-in example | `strlen($s)` | `sort($a)`, `array_push($a, ...)` |

> **Exam Tip:** "Differentiate between call by value and call by reference with example" is one of the most frequently asked questions in this unit. Write both functions, call them on the same starting value, and show the different outputs, exactly as in the example above.

#### Default parameter values

A parameter can have a default value. If the caller does not pass that argument, the default is used. Parameters with defaults should be placed **after** required parameters.

```php
<?php
// Example: Default parameter values
function calculatePrice($amount, $discountPercent = 0, $vatPercent = 13)
{
    $afterDiscount = $amount - ($amount * $discountPercent / 100);
    return $afterDiscount + ($afterDiscount * $vatPercent / 100);
}

echo calculatePrice(1000) . "\n";          // no discount, 13% VAT
echo calculatePrice(1000, 10) . "\n";      // 10% discount, 13% VAT
echo calculatePrice(1000, 10, 0) . "\n";   // 10% discount, no VAT
```

**Output:**
```
1130
1017
900
```

#### Named arguments (PHP 8)

PHP 8 lets you pass arguments by **parameter name**. You can skip optional parameters and pass them in any order.

```php
<?php
// Example: Named arguments
function calculatePrice($amount, $discountPercent = 0, $vatPercent = 13)
{
    $afterDiscount = $amount - ($amount * $discountPercent / 100);
    return $afterDiscount + ($afterDiscount * $vatPercent / 100);
}

// skip discount, set only VAT
echo calculatePrice(1000, vatPercent: 0) . "\n";

// any order
echo calculatePrice(vatPercent: 13, amount: 2000, discountPercent: 50) . "\n";

// works with built-in functions too
echo str_pad("7", 3, pad_type: STR_PAD_LEFT, pad_string: "0") . "\n";
```

**Output:**
```
1000
1130
007
```

#### Type declarations

You can declare the **type** of each parameter and of the return value. PHP then checks the types and reports errors early.

```php
<?php
// Example: Type declarations
declare(strict_types=1);   // must be the first statement in the file

function percentage(int $obtained, int $total): float
{
    return $obtained / $total * 100;
}

function fullName(string $first, ?string $middle, string $last): string
{
    // ?string means "string or null"
    return $middle === null ? "$first $last" : "$first $middle $last";
}

function logMessage(string $msg): void
{
    echo "[LOG] $msg\n";    // void: returns nothing
}

echo percentage(450, 500) . "\n";
echo fullName("Ram", null, "Thapa") . "\n";
echo fullName("Sita", "Kumari", "Rai") . "\n";
logMessage("Done");
```

**Output:**
```
90
Ram Thapa
Sita Kumari Rai
[LOG] Done
```

Common types: `int`, `float`, `string`, `bool`, `array`, `callable`, `?type` (nullable), `void` (return only), `mixed`, and union types (PHP 8) like `int|float`.

Without `declare(strict_types=1);` PHP converts compatible values automatically (for example `"5"` becomes `5`). With strict types on, `percentage("450", 500)` throws a `TypeError`. Strict mode catches bugs early and is recommended for new code.

#### Returning multiple values via an array

A function can return only **one** value, but that value can be an array holding many results. Use destructuring `[...] = ` to unpack it.

```php
<?php
// Example: Return multiple values using an array
function analyseMarks(array $marks): array
{
    $total = array_sum($marks);
    return [
        "total"   => $total,
        "average" => round($total / count($marks), 2),
        "highest" => max($marks),
    ];
}

$result = analyseMarks([78, 65, 90, 55]);
echo "Total: {$result['total']}, Average: {$result['average']}\n";

// destructuring with keys
['highest' => $top, 'average' => $avg] = analyseMarks([60, 70, 80]);
echo "Highest: $top, Average: $avg\n";

// indexed return + list-style destructuring
function minMax(array $a): array
{
    return [min($a), max($a)];
}
[$low, $high] = minMax([12, 5, 30]);
echo "Min: $low, Max: $high\n";
```

**Output:**
```
Total: 288, Average: 72
Highest: 80, Average: 70
Min: 5, Max: 30
```

`list($low, $high) = minMax(...)` is the older equivalent of `[$low, $high] = ...`.

#### Variadic functions (variable number of arguments)

A parameter written with `...` (the **splat** operator) collects any number of arguments into an array.

```php
<?php
// Example: Variadic function
function sumAll(int ...$numbers): int
{
    return array_sum($numbers);
}

echo sumAll(5, 10) . "\n";
echo sumAll(1, 2, 3, 4, 5) . "\n";
echo sumAll() . "\n";

function introduce(string $teacher, string ...$subjects): string
{
    return "$teacher teaches " . implode(", ", $subjects);
}
echo introduce("Mr. Karki", "PHP", "Java") . "\n";

$values = [10, 20, 30];
echo sumAll(...$values) . "\n";   // spread an array into arguments
```

**Output:**
```
15
15
0
Mr. Karki teaches PHP, Java
60
```

The variadic parameter must be the **last** parameter.

#### Recursion

A **recursive function** is a function that calls itself. Every recursive function needs:

1. A **base case** that stops the recursion.
2. A **recursive case** that moves closer to the base case.

```php
<?php
// Example: Factorial using recursion
function factorial(int $n): int
{
    if ($n <= 1) {          // base case
        return 1;
    }
    return $n * factorial($n - 1);   // recursive case
}

echo "5! = " . factorial(5) . "\n";
echo "0! = " . factorial(0) . "\n";
```

**Output:**
```
5! = 120
0! = 1
```

How `factorial(5)` is evaluated:

```
factorial(5) = 5 * factorial(4)
             = 5 * 4 * factorial(3)
             = 5 * 4 * 3 * factorial(2)
             = 5 * 4 * 3 * 2 * factorial(1)
             = 5 * 4 * 3 * 2 * 1          <- base case reached
             = 120
```

```php
<?php
// Example: Fibonacci series and sum of digits using recursion
function fibonacci(int $n): int
{
    if ($n < 2) return $n;
    return fibonacci($n - 1) + fibonacci($n - 2);
}

function sumOfDigits(int $n): int
{
    if ($n < 10) return $n;
    return ($n % 10) + sumOfDigits(intdiv($n, 10));
}

for ($i = 0; $i < 10; $i++) {
    echo fibonacci($i) . " ";
}
echo "\n";
echo "Sum of digits of 4567: " . sumOfDigits(4567) . "\n";
```

**Output:**
```
0 1 1 2 3 5 8 13 21 34 
Sum of digits of 4567: 22
```

> **Common Mistake:** Forgetting the base case, or writing a recursive call that never reaches it (e.g. `factorial($n + 1)`). The function calls itself forever until PHP runs out of memory and stops with a fatal error. Always check: "Does every call get closer to the base case?"

> **Teacher's Note:** Trace `factorial(4)` on the board as a stack of boxes, pushing a box on each call and popping it on each return. Then show that a simple `for` loop can do the same job. Students should understand that recursion is natural for problems that contain smaller copies of themselves (factorial, Fibonacci, nested menus, folder trees). (Time: about 20 minutes for all of 3.3.1 examples.)

### 3.3.2 Variable Scoping

The **scope** of a variable is the part of the program where the variable can be accessed. PHP's scope rules are different from JavaScript, and this often surprises students.

```
  +-------------------- Global scope (main script) --------------------+
  |  $college = "PU";                                                   |
  |                                                                     |
  |   +---------- function show() ----------+                           |
  |   |  Local scope                        |                           |
  |   |  $college is NOT visible here       |                           |
  |   |  unless: global $college;           |                           |
  |   |       or $GLOBALS['college']        |                           |
  |   +-------------------------------------+                           |
  |                                                                     |
  |  Superglobals ($_GET, $_POST, $_SESSION, $GLOBALS ...)              |
  |  are visible EVERYWHERE                                             |
  +---------------------------------------------------------------------+
```

#### Local scope

A variable created **inside a function** is **local** to that function. It exists only while the function runs and is not visible outside.

```php
<?php
// Example: Local variable
function setCourse()
{
    $course = "BCSIT";      // local variable
    echo "Inside: $course\n";
}

setCourse();
echo "Outside: " . ($course ?? "undefined") . "\n";
```

**Output:**
```
Inside: BCSIT
Outside: undefined
```

#### Global scope

A variable created **outside all functions** is a **global** variable. In PHP (unlike JavaScript), global variables are **not automatically visible inside functions**.

```php
<?php
// Example: Global variable is not visible inside a function
$university = "Pokhara University";

function showUniversity()
{
    echo "University: " . ($university ?? "not visible") . "\n";
}

showUniversity();
```

**Output:**
```
University: not visible
```

(Without the `??`, PHP would show "Warning: Undefined variable $university".)

#### The `global` keyword

`global $var;` inside a function links the local name to the global variable.

```php
<?php
// Example: global keyword
$visitors = 100;

function newVisitor()
{
    global $visitors;
    $visitors++;
}

newVisitor();
newVisitor();
echo "Visitors: $visitors\n";
```

**Output:**
```
Visitors: 102
```

#### The `$GLOBALS` array

`$GLOBALS` is a superglobal associative array that contains **all global variables**. The key is the variable name without `$`.

```php
<?php
// Example: $GLOBALS array
$x = 10;
$y = 20;

function addGlobals()
{
    $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}

addGlobals();
echo "z = $z\n";
```

**Output:**
```
z = 30
```

> **Common Mistake:** Using `global` everywhere to share data between functions. It makes code hard to understand and test, because any function might change the variable. The better practice is to **pass values as parameters and return results**.
> ```php
> // Avoid                              // Prefer
> function total() {                    function total(array $cart): float {
>     global $cart;                         return array_sum($cart);
>     return array_sum($cart);          }
> }                                     echo total($cart);
> ```

#### Static variables

Normally a local variable is destroyed when the function ends, and it starts fresh on the next call. A **static** variable keeps its value **between calls**, but is still only visible inside the function.

```php
<?php
// Example: Normal local vs static variable
function normalCounter()
{
    $count = 0;
    $count++;
    return $count;
}

function staticCounter()
{
    static $count = 0;   // initialised only once
    $count++;
    return $count;
}

echo normalCounter() . normalCounter() . normalCounter() . "\n";
echo staticCounter() . staticCounter() . staticCounter() . "\n";
```

**Output:**
```
111
123
```

Use case: counting how many times a function is called, generating serial numbers, or caching a result that is expensive to compute.

#### Comparison of scopes

| Scope | Where declared | Where visible | Lifetime |
|-------|----------------|---------------|----------|
| Local | Inside a function | Only in that function | Until the function returns |
| Global | Outside all functions | Main script; in functions only via `global` / `$GLOBALS` | Until the script ends |
| Static | Inside a function with `static` | Only in that function | Until the script ends (value kept between calls) |
| Superglobal | Built into PHP | Everywhere, including all functions | Whole request |

#### Superglobals: an introduction

**Superglobals** are built-in arrays that are always available in **every scope**, without `global`. You will use them throughout the rest of the course.

| Superglobal | Contains | Covered in |
|-------------|----------|-----------|
| `$GLOBALS` | All global variables | This unit |
| `$_GET` | Data sent in the URL query string | Form handling |
| `$_POST` | Data sent in the HTTP request body (forms with `method="post"`) | Form handling |
| `$_REQUEST` | `$_GET` + `$_POST` + `$_COOKIE` (avoid; be explicit) | Form handling |
| `$_SERVER` | Server and request info (`REQUEST_METHOD`, `PHP_SELF`, ...) | Form handling |
| `$_SESSION` | Session data for the current user | Sessions |
| `$_COOKIE` | Cookies sent by the browser | Cookies |
| `$_FILES` | Uploaded file information | File upload |
| `$_ENV` | Environment variables | - |

```php
<?php
// Example: Superglobals are visible inside functions
function requestInfo(): string
{
    // no "global" needed
    $method = $_SERVER['REQUEST_METHOD'] ?? 'CLI';
    return "Request method: $method";
}

echo requestInfo() . "\n";
```

**Output (when run from the command line; in a browser it shows GET):**
```
Request method: CLI
```

Remember: data in `$_GET`, `$_POST`, `$_COOKIE` and `$_FILES` comes from the user and **must never be trusted**. Validate it with `filter_input()` / `filter_var()` and escape it with `htmlspecialchars()` before output.

#### Anonymous functions and arrow functions (briefly)

An **anonymous function** (closure) is a function without a name. It can be stored in a variable or passed to another function such as `array_map()` or `usort()`.

Anonymous functions also have their **own scope**. To use a variable from the outer scope, import it with `use`.

```php
<?php
// Example: Anonymous function with use, and arrow function
$vatRate = 0.13;

// anonymous function: outer variable imported with use
$addVat = function ($price) use ($vatRate) {
    return $price + $price * $vatRate;
};
echo $addVat(1000) . "\n";

// arrow function (PHP 7.4+): outer variables are captured automatically
$addVat2 = fn($price) => $price + $price * $vatRate;
echo $addVat2(2000) . "\n";

// typical use: as a callback
$prices = [100, 200];
print_r(array_map(fn($p) => $p * (1 + $vatRate), $prices));
```

**Output:**
```
1130
2260
Array
(
    [0] => 113
    [1] => 226
)
```

| Feature | Anonymous function `function() use () {}` | Arrow function `fn() =>` |
|---------|--------------------------------------------|--------------------------|
| PHP version | 5.3+ | 7.4+ |
| Body | Many statements, needs `return` | **Single expression**, returned automatically |
| Outer variables | Must list them with `use ($x)` | Captured automatically (by value) |
| Modify outer variable | Possible with `use (&$x)` | Not possible |

Note: `use ($x)` copies the value **at the time the function is created**. If `$x` changes later, the closure still has the old value, unless you use `use (&$x)`.

> **Teacher's Note:** Students who know JavaScript expect functions to "see" outer variables automatically. Run the "global variable not visible" example live first and let them be surprised; then show the three fixes (`global`, `$GLOBALS`, passing as a parameter) and explain why parameters are best. Mention that arrow functions are the one place PHP behaves like JavaScript.

> **Exam Tip:** "What is variable scope? Explain local, global and static variables with example" is a classic 5-mark question. Write one short program per scope with output, then finish with the comparison table.

---

## Summary

- An **array** stores many values in one variable as ordered `key => value` pairs; PHP arrays grow automatically.
- **Indexed arrays** use integer keys starting at 0; **associative arrays** use string keys; **multi-dimensional arrays** store arrays inside arrays (e.g. rows of student records).
- `foreach ($arr as $key => $value)` is the standard way to iterate any array; `for` and `while` work for continuously indexed arrays. Use `&$value` (and `unset()` afterwards) to modify elements in a `foreach`.
- `print_r()` shows structure; `var_dump()` also shows types and lengths. Both are for debugging; wrap them in `<pre>` in a browser.
- A **function** is a named, reusable block of code; built-in functions come with PHP, user-defined functions are written with the `function` keyword.
- Key string functions: `strlen, str_word_count, strrev, strpos, str_replace, substr, strtoupper, strtolower, ucfirst, ucwords, trim, explode, implode, sprintf, number_format, str_contains`. Compare `strpos()` with `=== false`.
- Key math functions: `abs, round, ceil, floor, sqrt, pow, max, min, intdiv, fmod, pi`. Use `random_int()` (not `rand()`) for anything security-related.
- Always set `date_default_timezone_set('Asia/Kathmandu')`. Use `date()` with format characters, `mktime()`, `strtotime()`, `checkdate()`, and `DateTime::diff()` for differences such as age.
- Sorting: `sort/rsort` re-index; `asort/arsort` sort by value keeping keys; `ksort/krsort` sort by key. All modify the original array.
- `array_map()` transforms every element, `array_filter()` keeps matching elements, `array_slice()` copies a part, `array_splice()` changes the original.
- Arguments are passed **by value** by default; use `&` to pass **by reference**. Default values, named arguments (PHP 8), type declarations, variadic `...` parameters and returning arrays make functions flexible.
- A **recursive** function calls itself and must have a base case.
- Function variables are **local**. Globals are reached with `global` or `$GLOBALS` (prefer parameters). **Static** variables keep their value between calls. **Superglobals** are visible everywhere.
- Anonymous functions import outer variables with `use`; arrow functions `fn() =>` capture them automatically.

## Practice Questions

### Short Answer Questions (2-3 marks)

1. What is an array? Why are arrays needed in PHP? Give an example.
2. Differentiate between `print_r()` and `var_dump()` with an example.
3. What is the output of `echo strpos("PHP is fun", "PHP");`? Why is `if (strpos(...))` dangerous here?
4. Explain `explode()` and `implode()` with one example each.
5. Write the PHP statement to display the current date in the format `Sunday, 22-Nov-2026`.
6. Differentiate between `sort()`, `asort()` and `ksort()`.
7. What is the difference between `array_slice()` and `array_splice()`?
8. What are default parameters? Write a function with one default parameter.
9. What is a static variable? How is it different from a normal local variable?
10. What is a superglobal? List any four superglobals.

### Long Answer Questions (5-10 marks)

1. Explain indexed, associative and multi-dimensional arrays with suitable examples. Describe different ways to iterate through an array in PHP.
2. Explain any eight string functions in PHP with syntax and example.
3. Explain the date and time functions `date()`, `mktime()`, `strtotime()` and `checkdate()` with examples. Write a PHP program to calculate a person's age in years, months and days from the date of birth.
4. Differentiate between call by value and call by reference with suitable PHP examples. Also explain default arguments and named arguments.
5. What is variable scope? Explain local, global and static scope with examples. How can a function access a global variable?
6. What is recursion? Write a recursive PHP function to find the factorial of a number and explain how it works step by step.

### Programming Exercises

1. **(Easy)** Create an indexed array of 7 days of the week and print them using `for`, `foreach` and `while` loops.
2. **(Easy)** Create an associative array of 5 products and their prices. Print each product with its price, then print the total and the most expensive product.
3. **(Easy)** Write a program that takes a sentence and prints its length, number of words, the sentence in uppercase, and the sentence reversed.
4. **(Medium)** Write a function `isPalindrome(string $s): bool` that ignores case and spaces (e.g. "Nurses Run" is a palindrome).
5. **(Medium)** Given `$marks = ["Ram" => 78, "Sita" => 92, "Hari" => 65, "Gita" => 88];` print a rank list from highest to lowest with rank numbers, and print the names of students scoring above the average.
6. **(Medium)** Write a function `daysUntil(string $date): int` that returns the number of days from today to the given date, using `DateTime` and `diff()`.
7. **(Hard)** Store name and marks in 5 subjects of 5 students in a multi-dimensional array. Using user-defined functions, calculate total, percentage and grade for each student, and display the result in an HTML table sorted by percentage (highest first).
8. **(Hard)** Write a recursive function that prints a nested menu array (menu items with optional `children` arrays) as nested HTML `<ul>` lists.

## Lab Work

### Lab 1: Student Result Sheet with Arrays and Functions

**Objective:** Use multi-dimensional arrays, built-in array functions and user-defined functions to produce a result sheet.

**Steps:**

1. Create `result.php` in your `htdocs` folder (XAMPP).
2. Store at least 5 students in an array; each student has `name`, `roll` and `marks` (an associative array of 4 subjects).
3. Write functions:
   - `calculateTotal(array $marks): int` using `array_sum()`
   - `calculatePercentage(int $total, int $subjects): float`
   - `getGrade(float $percent): string`
   - `hasPassed(array $marks): bool` (every subject at least 40, use `min()`)
4. Add total, percentage, grade and result to each student, then sort the students by percentage in descending order using `usort()`.
5. Display the data in an HTML table using `htmlspecialchars()` for names. Show the date of report generation with `date("d F Y")` after setting the timezone to `Asia/Kathmandu`.

**Expected outcome:** A browser page with a sorted result table, correct totals, percentages (2 decimals), grades and Pass/Fail status, and today's date.

**Reference solution:**

```php
<?php
declare(strict_types=1);
date_default_timezone_set('Asia/Kathmandu');

$students = [
    ["roll" => 1, "name" => "Aarav Shrestha", "marks" => ["PHP" => 78, "Java" => 85, "DBMS" => 72, "Stats" => 66]],
    ["roll" => 2, "name" => "Sita Gurung",    "marks" => ["PHP" => 92, "Java" => 88, "DBMS" => 95, "Stats" => 90]],
    ["roll" => 3, "name" => "Bikash Thapa",   "marks" => ["PHP" => 45, "Java" => 38, "DBMS" => 60, "Stats" => 52]],
    ["roll" => 4, "name" => "Gita Adhikari",  "marks" => ["PHP" => 81, "Java" => 74, "DBMS" => 79, "Stats" => 85]],
    ["roll" => 5, "name" => "Rohan Karki",    "marks" => ["PHP" => 55, "Java" => 61, "DBMS" => 48, "Stats" => 70]],
];

function calculateTotal(array $marks): int
{
    return array_sum($marks);
}

function calculatePercentage(int $total, int $subjects): float
{
    return $total / ($subjects * 100) * 100;
}

function getGrade(float $percent): string
{
    return match (true) {
        $percent >= 90 => "A+",
        $percent >= 80 => "A",
        $percent >= 70 => "B+",
        $percent >= 60 => "B",
        $percent >= 40 => "C",
        default        => "F",
    };
}

function hasPassed(array $marks): bool
{
    return min($marks) >= 40;
}

foreach ($students as &$s) {
    $s["total"]   = calculateTotal($s["marks"]);
    $s["percent"] = calculatePercentage($s["total"], count($s["marks"]));
    $s["passed"]  = hasPassed($s["marks"]);
    $s["grade"]   = $s["passed"] ? getGrade($s["percent"]) : "F";
}
unset($s);

usort($students, fn($a, $b) => $b["percent"] <=> $a["percent"]);
$subjects = array_keys($students[0]["marks"]);
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Result Sheet</title>
</head>
<body>
    <h2>BCSIT Semester III Result Sheet</h2>
    <p>Generated on: <?= date("d F Y") ?></p>
    <table border="1" cellpadding="6">
        <tr>
            <th>Rank</th><th>Roll</th><th>Name</th>
            <?php foreach ($subjects as $sub): ?>
                <th><?= htmlspecialchars($sub) ?></th>
            <?php endforeach; ?>
            <th>Total</th><th>%</th><th>Grade</th><th>Result</th>
        </tr>
        <?php foreach ($students as $rank => $s): ?>
        <tr>
            <td><?= $rank + 1 ?></td>
            <td><?= $s["roll"] ?></td>
            <td><?= htmlspecialchars($s["name"]) ?></td>
            <?php foreach ($s["marks"] as $m): ?>
                <td><?= $m ?></td>
            <?php endforeach; ?>
            <td><?= $s["total"] ?></td>
            <td><?= number_format($s["percent"], 2) ?></td>
            <td><?= $s["grade"] ?></td>
            <td><?= $s["passed"] ? "Pass" : "Fail" ?></td>
        </tr>
        <?php endforeach; ?>
    </table>
</body>
</html>
```

**Expected table (in the browser):**

| Rank | Roll | Name | PHP | Java | DBMS | Stats | Total | % | Grade | Result |
|------|------|------|-----|------|------|-------|-------|---|-------|--------|
| 1 | 2 | Sita Gurung | 92 | 88 | 95 | 90 | 365 | 91.25 | A+ | Pass |
| 2 | 4 | Gita Adhikari | 81 | 74 | 79 | 85 | 319 | 79.75 | B+ | Pass |
| 3 | 1 | Aarav Shrestha | 78 | 85 | 72 | 66 | 301 | 75.25 | B+ | Pass |
| 4 | 5 | Rohan Karki | 55 | 61 | 48 | 70 | 234 | 58.50 | C | Pass |
| 5 | 3 | Bikash Thapa | 45 | 38 | 60 | 52 | 195 | 48.75 | F | Fail |

### Lab 2: String and Date Utility Page

**Objective:** Practise string, math and date built-in functions and variable scope.

**Steps:**

1. Create `utils.php`. Set the timezone to `Asia/Kathmandu`.
2. Store a sentence in a variable (for example "  pokhara university offers bcsit  ") and display: the trimmed sentence, its length, word count, `ucwords()` version, reversed version, and the position of the word "bcsit".
3. Use `explode()` to split the sentence into words, sort the words alphabetically and join them back with `implode(", ", ...)`.
4. Display today's date in three formats (`Y-m-d`, `d/m/Y`, `l, d F Y h:i A`), and the number of days left until 31 December of this year using `DateTime::diff()`.
5. Write a function `pageViews()` with a `static` counter and call it three times; print the value each time. Then rewrite it using `global` and explain in a comment which version is better and why.
6. Generate a 6-digit OTP using `random_int()` and display it formatted with `sprintf("%06d", ...)`.

**Expected outcome:** A page showing each result clearly labelled. Students should be able to explain why `random_int()` is used, why the timezone is set, and how the `static` counter keeps its value between calls.
