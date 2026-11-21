# Unit 2: Control Structures and Loop
**Course:** Internet Technology II (CMP 273) · **Teaching hours:** 5

## Learning Objectives

After this unit, you will be able to:

- Explain what a conditional statement is and how it controls the flow of a PHP program.
- Write `if`, `if...else`, `if...elseif...else` and nested `if` statements, including the alternative (colon) syntax used inside HTML templates.
- Use the ternary operator `?:` as a short alternative to a simple `if...else`, and know the related null coalescing operator `??`.
- Write a `switch` statement correctly, using `break` and `default`, and explain fall-through.
- Use `while`, `do...while` and `for` loops to perform repetitive tasks, and choose the right loop for a problem.
- Write nested loops to print patterns and tables, and solve classic problems (factorial, prime, Fibonacci, multiplication table).
- Control loop flow with `break`, `continue`, and their numeric forms such as `break 2`.

---

## Introduction: What Is "Control Flow"?

By default, PHP runs statements **one after another, from top to bottom**. This is called **sequential execution**. Real programs need more than that:

- **Selection (decision making):** run some code only when a condition is true. Example: show "Pass" only if marks are 40 or more.
- **Iteration (repetition):** run the same code many times. Example: print a row for each of 48 students.

The statements that change the normal top-to-bottom order are called **control structures**. Every programming language has the same three basic building blocks:

| Building block | Meaning | PHP statements |
|---|---|---|
| Sequence | One statement after another | Normal statements |
| Selection | Choose a path based on a condition | `if`, `else`, `elseif`, `?:`, `switch` |
| Iteration | Repeat a block | `while`, `do...while`, `for`, `foreach` |

```
   Sequence            Selection                 Iteration
   --------            ---------                 ---------
   [ stmt 1 ]          < condition? >            +--> < condition? > --false--> exit
       |               /           \             |         | true
   [ stmt 2 ]       true          false          |     [ body ]
       |             |              |            |         |
   [ stmt 3 ]     [ A ]          [ B ]           +---------+
```

All conditions in PHP are evaluated as a **boolean** (`true` or `false`). You learned the comparison operators (`==`, `===`, `!=`, `<`, `>=`, ...) and logical operators (`&&`, `||`, `!`) in Unit 1; we use them heavily here.

> **Teacher's Note:** Start with a real decision students make every day: "If it is raining, take an umbrella, otherwise wear sunglasses." Then ask, "How do you write the same thing for a computer?" This makes the idea of a condition concrete before any syntax appears.

### Truthy and falsy values (quick recap)

A condition does not have to be a comparison. PHP converts any value to boolean. These values are treated as **false** ("falsy"):

| Value | Type |
|---|---|
| `false` | boolean |
| `0` and `0.0` | integer / float |
| `""` (empty string) and `"0"` | string |
| `[]` (empty array) | array |
| `null` | null |

Everything else is **true** ("truthy"), including `"false"`, `"0.0"`, `" "` (a space) and `-1`.

---

## 2.1 Conditional Statements

A **conditional statement** is a statement that checks a condition (a boolean expression) and decides which block of code to execute. It lets a program **make decisions** at run time, based on user input, database values, the time of day, and so on.

**Why it matters in web development:**

- Show a "Welcome, Sita" message if a user is logged in, otherwise show a login link.
- Validate a form: if the email is empty, show an error.
- Give a discount if the cart total is above Rs. 5000.
- Show a different menu for an admin and a normal user.

PHP provides three kinds of conditional constructs:

1. `if`, `else`, `elseif` (most flexible)
2. The ternary operator `?:` (short form for simple choices)
3. `switch` (compare one value against many fixed options)

### 2.1.1 If, Else, Elseif

#### (a) The `if` statement

The `if` statement executes a block **only when** the condition is true. If the condition is false, the block is skipped.

**Syntax:**

```php
if (condition) {
    // statements run only when condition is true
}
```

**Flowchart:**

```
        +-------------+
        |   Start     |
        +------+------+
               |
        < condition? >---- false ----+
               | true                |
        [ if-block ]                 |
               |                     |
               +<--------------------+
               |
        [ next statement ]
```

```php
<?php
// Example: Simple if - checking voting age
$name = "Ramesh";
$age  = 20;

if ($age >= 18) {
    echo "$name is eligible to vote.\n";
}

echo "Program finished.\n";
```

**Output:**
```
Ramesh is eligible to vote.
Program finished.
```

If `$age` were `15`, only `Program finished.` would print.

#### (b) The `if...else` statement

`if...else` chooses between **exactly two** paths. When the condition is true, the `if` block runs; otherwise the `else` block runs. Exactly one of the two blocks always runs.

**Syntax:**

```php
if (condition) {
    // runs when condition is true
} else {
    // runs when condition is false
}
```

**Flowchart:**

```
              < condition? >
             /              \
          true              false
           |                  |
     [ if-block ]      [ else-block ]
           \                  /
            +-------+--------+
                    |
           [ next statement ]
```

```php
<?php
// Example: Even or odd
$number = 17;

if ($number % 2 === 0) {
    echo "$number is even.\n";
} else {
    echo "$number is odd.\n";
}
```

**Output:**
```
17 is odd.
```

#### (c) The `if...elseif...else` ladder

When there are **more than two** possibilities, use `elseif`. PHP checks the conditions **from top to bottom** and runs the block of the **first** true condition. The rest are skipped. The final `else` (optional) runs if none of the conditions is true.

**Syntax:**

```php
if (condition1) {
    // block 1
} elseif (condition2) {
    // block 2
} elseif (condition3) {
    // block 3
} else {
    // runs if none of the above is true
}
```

**Flowchart:**

```
   < cond1? >--true--> [ block 1 ] ----------------------+
       | false                                           |
   < cond2? >--true--> [ block 2 ] ----------------------+
       | false                                           |
   < cond3? >--true--> [ block 3 ] ----------------------+
       | false                                           |
   [ else block ] ---------------------------------------+
                                                         |
                                                [ next statement ]
```

```php
<?php
// Example: Grade calculation (Pokhara University style letter grades, simplified)
$student = "Anisha Gurung";
$marks   = 76;

if ($marks < 0 || $marks > 100) {
    $grade = "Invalid marks";
} elseif ($marks >= 90) {
    $grade = "A";
} elseif ($marks >= 80) {
    $grade = "A-";
} elseif ($marks >= 70) {
    $grade = "B+";
} elseif ($marks >= 60) {
    $grade = "B";
} elseif ($marks >= 50) {
    $grade = "C";
} else {
    $grade = "F";
}

echo "$student scored $marks marks. Grade: $grade\n";
```

**Output:**
```
Anisha Gurung scored 76 marks. Grade: B+
```

Notice the **order** of conditions. Because PHP stops at the first true condition, we check the highest range first. `76 >= 90` is false, `76 >= 80` is false, `76 >= 70` is true, so grade `B+` is assigned and the remaining checks are skipped.

> **Common Mistake:** Writing the conditions in the wrong order.
> ```php
> if ($marks >= 50) { $grade = "C"; }
> elseif ($marks >= 90) { $grade = "A"; }   // never reached for 95!
> ```
> A student with 95 gets "C" because `95 >= 50` is true first. **Correct:** check the most restrictive (highest) range first, or use full ranges such as `$marks >= 50 && $marks < 60`.

> **Common Mistake:** Using `=` (assignment) instead of `==` or `===` (comparison).
> ```php
> if ($role = "admin") { ... }   // WRONG: assigns "admin", always truthy
> if ($role === "admin") { ... } // CORRECT
> ```

**`elseif` vs `else if`:** In PHP both work with curly braces. `elseif` is one keyword; `else if` is an `else` followed by a new `if`. With the colon (alternative) syntax, only `elseif` works. Prefer `elseif`.

#### (d) Nested `if`

A **nested if** is an `if` statement placed **inside** another `if` or `else` block. Use it when a second decision only makes sense after the first decision is true.

```php
<?php
// Example: Nested if - login check
$username   = "hari";
$password   = "secret123";
$isVerified = false;

if ($username === "hari") {
    if ($password === "secret123") {
        if ($isVerified) {
            echo "Login successful. Welcome, Hari!\n";
        } else {
            echo "Please verify your email first.\n";
        }
    } else {
        echo "Wrong password.\n";
    }
} else {
    echo "User not found.\n";
}
```

**Output:**
```
Please verify your email first.
```

(This example compares plain-text passwords only to demonstrate nesting. Real login code stores a hash with `password_hash()` and checks it with `password_verify()`; that is covered in the sessions/security unit.)

```php
<?php
// Example: Largest of three numbers using nested if
$a = 45;
$b = 78;
$c = 62;

if ($a > $b) {
    if ($a > $c) {
        $largest = $a;
    } else {
        $largest = $c;
    }
} else {
    if ($b > $c) {
        $largest = $b;
    } else {
        $largest = $c;
    }
}

echo "Largest of $a, $b and $c is $largest\n";
```

**Output:**
```
Largest of 45, 78 and 62 is 78
```

Deep nesting (3+ levels) is hard to read. Often you can flatten it with logical operators or **early exits**:

For example, `if ($age >= 18) { if ($hasLicense) { ... } }` is the same as `if ($age >= 18 && $hasLicense) { ... }`, which is easier to read.

#### (e) Leap year: a classic exam program

A year is a leap year if it is divisible by 4 **and not** by 100, **or** it is divisible by 400.

```php
<?php
// Example: Check leap year with if...elseif...else
$year = 2024;

if ($year % 400 === 0) {
    echo "$year is a leap year.\n";
} elseif ($year % 100 === 0) {
    echo "$year is not a leap year.\n";
} elseif ($year % 4 === 0) {
    echo "$year is a leap year.\n";
} else {
    echo "$year is not a leap year.\n";
}
```

**Output:**
```
2024 is a leap year.
```

The same rule in one condition: `if (($year % 4 === 0 && $year % 100 !== 0) || $year % 400 === 0)`.

#### (f) Using `if` with form input (web example)

In a real page the value comes from the user. Always validate input and escape output with `htmlspecialchars()`.

```php
<?php
// Example: age_check.php - decide based on form input
// Visit: age_check.php?name=Sita&age=17
$name = filter_input(INPUT_GET, 'name', FILTER_DEFAULT) ?? '';
$age  = filter_input(INPUT_GET, 'age', FILTER_VALIDATE_INT,
                     ['options' => ['min_range' => 1, 'max_range' => 120]]);

$safeName = htmlspecialchars(trim($name), ENT_QUOTES, 'UTF-8');

if ($safeName === '') {
    echo "<p>Please enter your name.</p>";
} elseif ($age === false || $age === null) {
    echo "<p>Please enter a valid age (1 to 120).</p>";
} elseif ($age >= 18) {
    echo "<p>Hello $safeName, you can create an account.</p>";
} else {
    echo "<p>Sorry $safeName, you must be 18 or older.</p>";
}
```

**Output** (for `?name=Sita&age=17`):
```html
<p>Sorry Sita, you must be 18 or older.</p>
```

`filter_input()` returns `null` if the field is missing and `false` if it fails validation, which is why both are checked.

#### (g) Alternative syntax for HTML templates

When PHP is mixed with a lot of HTML, curly braces become hard to match: a lone `<?php } ?>` far down the page tells you nothing. PHP offers an **alternative syntax** that replaces the opening brace `{` with a colon `:` and the closing brace `}` with an ending keyword.

| Normal syntax | Alternative syntax |
|---|---|
| `if (...) {` | `if (...):` |
| `} elseif (...) {` | `elseif (...):` |
| `} else {` | `else:` |
| `}` (end of if) | `endif;` |
| `while (...) { ... }` | `while (...): ... endwhile;` |
| `for (...) { ... }` | `for (...): ... endfor;` |
| `switch (...) { ... }` | `switch (...): ... endswitch;` |
| `foreach (...) { ... }` | `foreach (...): ... endforeach;` |

There is **no** alternative syntax for `do...while`.

```php
<?php
// Example: dashboard.php - alternative syntax inside HTML
$isLoggedIn = true;
$userName   = "Bikash Thapa";
$role       = "admin";
?>
<nav>
<?php if ($isLoggedIn): ?>
    <p>Welcome, <?= htmlspecialchars($userName) ?>!</p>
    <?php if ($role === "admin"): ?>
        <a href="admin.php">Admin Panel</a>
    <?php elseif ($role === "teacher"): ?>
        <a href="marks.php">Enter Marks</a>
    <?php else: ?>
        <a href="profile.php">My Profile</a>
    <?php endif; ?>
    <a href="logout.php">Logout</a>
<?php else: ?>
    <a href="login.php">Login</a>
<?php endif; ?>
</nav>
```

**Output** (HTML sent to the browser; PHP removes the newline right after each `?>`):
```html
<nav>
    <p>Welcome, Bikash Thapa!</p>
            <a href="admin.php">Admin Panel</a>
        <a href="logout.php">Logout</a>
</nav>
```

The browser ignores the extra spaces, so the page shows "Welcome, Bikash Thapa!", an "Admin Panel" link and a "Logout" link.

`<?= ... ?>` is the short echo tag. It is the same as `<?php echo ...; ?>` and is always available in PHP 5.4+.

> **Teacher's Note:** Show the same page written twice, once with braces and once with the colon syntax. Ask students to find the matching close for the outer `if` in each version. The `endif;` version wins every time. Mention that template engines (Blade in Laravel, Twig) use the same idea.

> **Common Mistake:** Mixing the two styles, for example `if ($x): ... }` or forgetting the semicolon in `endif;`. Each `if (...):` must end with `endif;`. Also, `else if` (two words) does not work with the colon syntax; write `elseif`.

> **Exam Tip:** A frequent short question is "What is the alternative syntax of control structures in PHP? Why is it used?" Answer: colon in place of `{`, and `endif;` / `endwhile;` / `endfor;` / `endforeach;` / `endswitch;` in place of `}`; it makes PHP embedded in HTML templates easier to read. Give a 5-line example.

### 2.1.2 Ternary Operator

The **ternary operator** `?:` is a **conditional operator** that takes **three operands**. It evaluates a condition and returns one of two values. It is a compact replacement for a simple `if...else` that **assigns or outputs a value**.

**Syntax:**

```php
$result = condition ? value_if_true : value_if_false;
```

```
   condition  ?  expr1  :  expr2
       |           |         |
   evaluated   returned   returned
     first     if true    if false
```

```php
<?php
// Example: if...else vs ternary
$marks = 35;

// Using if...else
if ($marks >= 40) {
    $result = "Pass";
} else {
    $result = "Fail";
}
echo "if...else: $result\n";

// Using the ternary operator - same logic in one line
$result = ($marks >= 40) ? "Pass" : "Fail";
echo "Ternary:   $result\n";

// Directly inside echo
$items = 1;
echo "You have $items item" . ($items === 1 ? "" : "s") . " in your cart.\n";
```

**Output:**
```
if...else: Fail
Ternary:   Fail
You have 1 item in your cart.
```

**Ternary in HTML** (very common for CSS classes and selected options):

```php
<?php
// Example: Ternary inside HTML attributes
$marks = 82;
$city  = "Pokhara";
?>
<span class="<?= $marks >= 40 ? 'pass' : 'fail' ?>"><?= $marks ?></span>
<select name="city">
    <option <?= $city === "Kathmandu" ? "selected" : "" ?>>Kathmandu</option>
    <option <?= $city === "Pokhara" ? "selected" : "" ?>>Pokhara</option>
</select>
```

**Output:**
```html
<span class="pass">82</span>
<select name="city">
    <option >Kathmandu</option>
    <option selected>Pokhara</option>
</select>
```

#### Short ternary (Elvis operator) `?:`

If you leave out the middle part, `$a ?: $b` returns `$a` if `$a` is **truthy**, otherwise `$b`.

```php
<?php
// Example: Short ternary ?:
$nickname = "";
echo ($nickname ?: "Guest") . "\n";   // "" is falsy -> "Guest"

$nickname = "Rocky";
echo ($nickname ?: "Guest") . "\n";   // "Rocky" is truthy
```

**Output:**
```
Guest
Rocky
```

#### Related: the null coalescing operator `??` (PHP 7+)

`$a ?? $b` returns `$a` if it **exists and is not `null`**, otherwise `$b`. It does **not** raise a warning for an undefined variable or array key, so it is perfect for reading `$_GET`, `$_POST` and `$_SESSION` values.

```php
<?php
// Example: Null coalescing operator ??
$_GET = ['page' => '3'];         // simulate a URL like list.php?page=3

$page = $_GET['page'] ?? 1;      // key exists -> "3"
$sort = $_GET['sort'] ?? 'name'; // key missing -> default 'name', no warning

echo "Page: $page, Sort: $sort\n";

// Old way (still works, but longer)
$sort2 = isset($_GET['sort']) ? $_GET['sort'] : 'name';
echo "Old way: $sort2\n";

// Chaining: first non-null value wins
$fromForm = null;
$fromCookie = null;
$theme = $fromForm ?? $fromCookie ?? 'light';
echo "Theme: $theme\n";

// Null coalescing assignment ??= (PHP 7.4+)
$config = [];
$config['lang'] ??= 'en';
echo "Lang: {$config['lang']}\n";
```

**Output:**
```
Page: 3, Sort: name
Old way: name
Theme: light
Lang: en
```

#### `?:` vs `??` comparison

| Feature | Short ternary `$a ?: $b` | Null coalescing `$a ?? $b` |
|---|---|---|
| Returns `$b` when `$a` is | falsy (`0`, `""`, `"0"`, `[]`, `null`, `false`) | `null` or not set |
| Warning if `$a` undefined? | Yes ("Undefined variable/array key") | No |
| `0 ?: 5` / `0 ?? 5` | `5` | `0` |
| `"" ?: "x"` / `"" ?? "x"` | `"x"` | `""` |
| Typical use | Replace empty values | Read optional input with a default |

> **Common Mistake:** Nesting ternaries without parentheses.
> ```php
> echo $m >= 80 ? "A" : $m >= 60 ? "B" : "C";   // Fatal error in PHP 8
> ```
> PHP 8 refuses unparenthesised nested ternaries. Either add parentheses, `$m >= 80 ? "A" : ($m >= 60 ? "B" : "C")`, or (better) use `if...elseif`. A ternary should stay short and readable.

> **Exam Tip:** For "Explain the ternary operator with an example", write the syntax, a 3-line `if...else`, the same thing as a one-line ternary, and its output. Mention that it is called "ternary" because it has three operands. Adding one line about `??` earns extra credit.

### 2.1.3 Switch Statement

The **switch** statement compares **one expression** against a list of **case values** and runs the code for the matching case. It is a cleaner alternative to a long `if...elseif` chain when you test the **same variable** for **equality** with many fixed values.

**Syntax:**

```php
switch (expression) {
    case value1:
        // statements
        break;
    case value2:
        // statements
        break;
    default:
        // runs when no case matches
}
```

**How it works:**

1. The expression is evaluated **once**.
2. Its value is compared with each `case` value from top to bottom using **loose comparison (`==`)**.
3. When a match is found, execution starts at that `case` and continues **until a `break`** (or the end of the switch).
4. If no case matches, the `default` block runs (if present).

**Flowchart:**

```
          [ evaluate expression ]
                    |
           < == value1? >--yes--> [ case 1 ] --break--+
                    | no                              |
           < == value2? >--yes--> [ case 2 ] --break--+
                    | no                              |
           < == value3? >--yes--> [ case 3 ] --break--+
                    | no                              |
              [ default ] ----------------------------+
                                                      |
                                              [ after switch ]
```

```php
<?php
// Example: Day name from day number
$day = 3;

switch ($day) {
    case 1:
        echo "Sunday\n";
        break;
    case 2:
        echo "Monday\n";
        break;
    case 3:
        echo "Tuesday\n";
        break;
    // ... cases 4 to 6 follow the same pattern ...
    case 7:
        echo "Saturday\n";
        break;
    default:
        echo "Invalid day number\n";
}
```

**Output:**
```
Tuesday
```

(In Nepal the week starts on Sunday, so day 1 is Sunday.)

#### The `break` keyword and fall-through

`break` ends the switch. If you **leave out `break`**, PHP does not stop at the end of the matched case; it "falls through" and runs the next case's statements too, **without checking** their values.

```php
<?php
// Example: Accidental fall-through (missing break)
$grade = "B";

switch ($grade) {
    case "A":
        echo "Excellent\n";
    case "B":
        echo "Good\n";
    case "C":
        echo "Average\n";
    default:
        echo "Keep trying\n";
}
```

**Output:**
```
Good
Average
Keep trying
```

The match was `"B"`, but because there is no `break`, PHP also ran the `"C"` and `default` blocks.

#### Intentional fall-through: grouping cases

Fall-through is useful when **several values share the same code**. Stack the cases with no statements between them.

```php
<?php
// Example: Grouping cases - vowel or consonant
$letter = "e";

switch (strtolower($letter)) {
    case "a":
    case "e":
    case "i":
    case "o":
    case "u":
        echo "'$letter' is a vowel.\n";
        break;
    default:
        echo "'$letter' is a consonant.\n";
}
```

**Output:**
```
'e' is a vowel.
```

#### The `default` case

- `default` runs when **no** case matches. It is optional but strongly recommended, for example to handle invalid input.
- It is usually written last. If it is last, it does not need a `break`.
- There should be only one `default` per switch.

#### A simple calculator with switch

```php
<?php
// Example: Calculator using switch
$a  = 20;
$b  = 4;
$op = "/";

switch ($op) {
    case "+":
        $result = $a + $b;
        break;
    case "-":
        $result = $a - $b;
        break;
    case "*":
        $result = $a * $b;
        break;
    case "/":
        $result = ($b != 0) ? $a / $b : "Error: division by zero";
        break;
    case "%":
        $result = ($b != 0) ? $a % $b : "Error: division by zero";
        break;
    default:
        $result = "Unknown operator";
}

echo "$a $op $b = $result\n";
```

**Output:**
```
20 / 4 = 5
```

#### switch(true) for ranges

`case` values are compared for equality, so `case $marks >= 80:` inside `switch ($marks)` does **not** work as expected. A known trick is `switch (true)`: each case expression is evaluated, and the first one equal to `true` runs. It works, but an `if...elseif` ladder is usually clearer for ranges.

```php
switch (true) {
    case $temp >= 35: echo "Very hot"; break;
    case $temp >= 25: echo "Warm";     break;
    default:          echo "Cool";
}
```

#### if...elseif vs switch

| Point | `if...elseif...else` | `switch` |
|---|---|---|
| Type of test | Any boolean expression (ranges, `&&`, `||`, function calls) | Equality of one expression with fixed values |
| Number of variables | Can test different variables in each branch | Tests one expression |
| Comparison used | Whatever you write (`==`, `===`, `<`, ...) | Loose `==` |
| Fall-through | Not possible | Happens if `break` is missing |
| Readability | Better for ranges and complex logic | Better for many exact values (menu choice, day number) |
| Default branch | `else` | `default` |

> **Common Mistake:** Forgetting that `switch` uses loose comparison. In PHP 8, `switch ("1e1")` matches `case 10:` because `"1e1" == 10` is true. When the type matters, use `if` with `===` or the PHP 8 `match` expression.

> **Teacher's Note:** Demonstrate fall-through live: write the grade example, run it with `break`, then delete the breaks one by one and let students predict the output before running. It sticks better than any explanation. Budget about 20 minutes for switch.

> **Beyond the syllabus: PHP 8 `match` expression.** `match` is a newer, stricter cousin of `switch`. It uses **strict comparison (`===`)**, has **no fall-through**, **returns a value**, and throws an `UnhandledMatchError` if no arm matches and there is no `default`.
> ```php
> $day = 7;
> $type = match ($day) {
>     1, 2, 3, 4, 5, 6 => "Working day",
>     7 => "Holiday",
>     default => "Invalid",
> };
> echo $type;   // Holiday
> ```
> You will see it in modern code, but for the exam focus on `switch`.

> **Exam Tip:** "Explain switch statement with example" and "Differentiate between if-else and switch" are common. Draw the flowchart, write a short program (day name or calculator), and explicitly explain the role of `break` and `default`. Mention fall-through by name.

---

## 2.2 Using Loop for Repetitive Tasks

A **loop** is a control structure that **executes a block of code repeatedly** as long as a given condition remains true. Each single execution of the loop body is called an **iteration**.

**Why loops matter:** without a loop, printing the numbers 1 to 100 needs 100 `echo` statements. With a loop it needs three lines. In web pages, loops are used to:

- Display every row of a database result in an HTML table.
- Build `<option>` lists (years 2000 to 2025, districts, etc.).
- Calculate totals, averages and statistics.
- Retry an operation (for example, generate a random code until it is unique).

**Every loop has four parts:**

| Part | Purpose | Example |
|---|---|---|
| Initialization | Give the loop variable its starting value | `$i = 1` |
| Condition | Checked to decide whether to run (again) | `$i <= 10` |
| Body | The statements that repeat | `echo $i;` |
| Update | Change the loop variable so the loop eventually ends | `$i++` |

If the update is missing, or never makes the condition false, you get an **infinite loop**.

PHP has four loops:

| Loop | Covered in |
|---|---|
| `while` | This unit |
| `do...while` | This unit |
| `for` | This unit |
| `foreach` | Unit 3 (arrays). It loops over each element of an array, e.g. `foreach ($students as $s) { ... }`. |

### 2.2.1 While, Do...While, For

#### (a) The `while` loop

`while` checks the condition **before** each iteration. If the condition is false at the start, the body runs **zero times**. It is called an **entry-controlled** (pre-test) loop.

**Syntax:**

```php
initialization;
while (condition) {
    // body
    update;
}
```

**Flowchart:**

```
        [ initialization ]
               |
               v
   +----> < condition? >---- false ----> [ exit loop ]
   |           | true
   |        [ body ]
   |           |
   |        [ update ]
   |           |
   +-----------+
```

```php
<?php
// Example: Print 1 to 5 using while
$i = 1;                 // initialization

while ($i <= 5) {       // condition
    echo "Count: $i\n"; // body
    $i++;               // update
}

echo "Loop ended. \$i is now $i\n";
```

**Output:**
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
Loop ended. $i is now 6
```

`while` is best when you **do not know in advance** how many times to repeat. Example: keep dividing a number by 10 until it becomes 0.

```php
<?php
// Example: Sum and reverse of digits using while
$number   = 4721;
$n        = $number;
$sum      = 0;
$reversed = 0;

while ($n > 0) {
    $digit    = $n % 10;               // last digit
    $sum     += $digit;
    $reversed = $reversed * 10 + $digit;
    $n        = intdiv($n, 10);        // remove last digit
}

echo "Number: $number\n";
echo "Sum of digits: $sum\n";
echo "Reversed: $reversed\n";
echo ($number === $reversed) ? "It is a palindrome.\n" : "It is not a palindrome.\n";
```

**Output:**
```
Number: 4721
Sum of digits: 14
Reversed: 1274
It is not a palindrome.
```

Trace table for the first two iterations:

| Iteration | `$n` before | `$digit` | `$sum` | `$reversed` | `$n` after |
|---|---|---|---|---|---|
| 1 | 4721 | 1 | 1 | 1 | 472 |
| 2 | 472 | 2 | 3 | 12 | 47 |
| 3 | 47 | 7 | 10 | 127 | 4 |
| 4 | 4 | 4 | 14 | 1274 | 0 |

> **Common Mistake:** Forgetting the update.
> ```php
> $i = 1;
> while ($i <= 5) {
>     echo $i;      // $i never changes -> infinite loop, page hangs
> }
> ```
> Always make sure something inside the loop moves the condition toward `false`. On a web server an infinite loop runs until `max_execution_time` (default 30 seconds) stops the script with a fatal error.

#### (b) The `do...while` loop

`do...while` runs the body **first** and checks the condition **after** each iteration. So the body always runs **at least once**, even when the condition is false from the start. It is an **exit-controlled** (post-test) loop.

**Syntax** (note the semicolon at the end):

```php
initialization;
do {
    // body
    update;
} while (condition);
```

**Flowchart:**

```
        [ initialization ]
               |
               v
   +------> [ body ]
   |           |
   |        [ update ]
   |           |
   +- true -< condition? >
               | false
               v
          [ exit loop ]
```

```php
<?php
// Example: while vs do...while when the condition is false at the start
$i = 10;

echo "while loop:\n";
while ($i < 5) {
    echo "  i = $i\n";
    $i++;
}

echo "do...while loop:\n";
$i = 10;
do {
    echo "  i = $i\n";
    $i++;
} while ($i < 5);
```

**Output:**
```
while loop:
do...while loop:
  i = 10
```

The `while` body never runs, but the `do...while` body runs once.

**Typical use:** a task that must happen at least once, such as showing a menu once, or generating a random code and repeating only if that code is already in use.

#### (c) The `for` loop

`for` puts the **initialization, condition and update in one line**. It is the best choice when you **know how many times** the loop should run (counter-controlled loop). Like `while`, it is entry-controlled.

**Syntax:**

```php
for (initialization; condition; update) {
    // body
}
```

**Order of execution:**

```
for ( (1) init ; (2) condition ; (4) update ) {
        (3) body
}

(1) runs once -> (2) check -> (3) body -> (4) update -> (2) check -> (3) body -> ... -> (2) false -> exit
```

**Flowchart:**

```
        [ initialization ]   (once)
               |
               v
   +----> < condition? >---- false ----> [ exit loop ]
   |           | true
   |        [ body ]
   |           |
   +------ [ update ]
```

```php
<?php
// Example: Sum of first N natural numbers using for
$n   = 10;
$sum = 0;

for ($i = 1; $i <= $n; $i++) {
    $sum += $i;
}

echo "Sum of 1 to $n = $sum\n";
```

**Output:**
```
Sum of 1 to 10 = 55
```

```php
<?php
// Example: Counting down and stepping by 2
echo "Countdown: ";
for ($i = 5; $i >= 1; $i--) {
    echo "$i ";
}
echo "Go!\n";

echo "Even numbers up to 20: ";
for ($i = 2; $i <= 20; $i += 2) {
    echo "$i ";
}
echo "\n";
```

**Output:**
```
Countdown: 5 4 3 2 1 Go!
Even numbers up to 20: 2 4 6 8 10 12 14 16 18 20 
```

**Flexible parts.** Each of the three parts is optional, and a part may hold several expressions separated by commas, for example `for ($i = 1, $j = 10; $i < $j; $i++, $j--)`. `for (;;) { ... }` with all parts empty is an infinite loop; it must be ended with `break` inside.

> **Common Mistake:** Putting a semicolon right after the `for` or `while` header.
> ```php
> for ($i = 1; $i <= 5; $i++);   // <- this semicolon is an empty body
> {
>     echo $i;                   // runs only once, prints 6
> }
> ```
> Remove the semicolon so the block belongs to the loop.

> **Common Mistake:** Off-by-one errors. `for ($i = 1; $i < 10; $i++)` runs 9 times (1 to 9), not 10. Decide whether you need `<` or `<=` and test the first and last values.

#### (d) Generating HTML with loops

This is the most common use of loops in PHP web pages.

```php
<?php
// Example: Year dropdown for a date-of-birth form (alternative syntax)
$currentYear = 2025;
?>
<label for="year">Birth year:</label>
<select name="year" id="year">
<?php for ($y = $currentYear; $y >= $currentYear - 4; $y--): ?>
    <option value="<?= $y ?>"><?= $y ?></option>
<?php endfor; ?>
</select>
```

**Output:**
```html
<label for="year">Birth year:</label>
<select name="year" id="year">
    <option value="2025">2025</option>
    <option value="2024">2024</option>
    <option value="2023">2023</option>
    <option value="2022">2022</option>
    <option value="2021">2021</option>
</select>
```

(In a real form you would loop back 60 or more years; 5 are shown to keep the output short.)

```php
<?php
// Example: Generating HTML table rows with striped colours
$rows = 4;
echo "<table border=\"1\">\n";
echo "  <tr><th>S.N.</th><th>Square</th><th>Cube</th></tr>\n";

for ($i = 1; $i <= $rows; $i++) {
    $class = ($i % 2 === 0) ? "even" : "odd";
    echo "  <tr class=\"$class\"><td>$i</td><td>" . ($i ** 2) . "</td><td>" . ($i ** 3) . "</td></tr>\n";
}

echo "</table>\n";
```

**Output:**
```html
<table border="1">
  <tr><th>S.N.</th><th>Square</th><th>Cube</th></tr>
  <tr class="odd"><td>1</td><td>1</td><td>1</td></tr>
  <tr class="even"><td>2</td><td>4</td><td>8</td></tr>
  <tr class="odd"><td>3</td><td>9</td><td>27</td></tr>
  <tr class="even"><td>4</td><td>16</td><td>64</td></tr>
</table>
```

When the data comes from a database (Unit 5), you will use `while ($row = $result->fetch_assoc()) { ... }` to print one table row per record. That is a `while` loop whose condition becomes false when there are no more rows.

> **Teacher's Note:** Open the browser's "View Page Source" after running an HTML-generating loop. Students often think PHP sends the loop to the browser. Seeing only plain HTML in the source makes the server-side idea click.

#### (e) Comparison of while, do...while and for

| Point | `while` | `do...while` | `for` |
|---|---|---|---|
| Type | Entry-controlled (pre-test) | Exit-controlled (post-test) | Entry-controlled (pre-test) |
| Condition checked | Before the body | After the body | Before the body |
| Minimum runs | 0 | 1 | 0 |
| Init / condition / update | Written separately | Written separately | All in the header |
| Semicolon after condition | No | Yes: `} while (cond);` | No |
| Best used when | Number of repetitions unknown (read until end, digits of a number) | Body must run at least once (menu, retry, input prompt) | Number of repetitions known (1 to N, table of a number) |
| Alternative syntax | `while: ... endwhile;` | None | `for: ... endfor;` |

> **Exam Tip:** "Differentiate between while and do...while loop with example" appears very often. Use the table above (at least 4 points), draw both flowcharts, and give the example where the condition is false at the start, because it shows the difference in output clearly.

#### (f) Nested loops

A **nested loop** is a loop inside another loop. For **each** iteration of the outer loop, the inner loop runs **completely**. If the outer loop runs `m` times and the inner loop runs `n` times, the inner body runs `m × n` times.

Think of a clock: the minute hand (inner loop) goes all the way round for every single step of the hour hand (outer loop).

**Rule for patterns:** the outer loop controls **rows** (lines), the inner loop controls **columns** (what is printed on each line).

```php
<?php
// Example: How nested loops run
for ($i = 1; $i <= 3; $i++) {          // outer loop: rows
    for ($j = 1; $j <= 2; $j++) {      // inner loop: columns
        echo "($i,$j) ";
    }
    echo "\n";                         // new line after each row
}
```

**Output:**
```
(1,1) (1,2) 
(2,1) (2,2) 
(3,1) (3,2) 
```

#### (g) Pattern programs

Patterns are a favourite exam question because they test understanding of nested loops. In each case, find the relation between the **row number** `$i` and **what is printed** in that row.

**Pattern 1: Right-angled triangle of stars** (row `i` has `i` stars)

```php
<?php
// Example: Right triangle
$n = 5;
for ($i = 1; $i <= $n; $i++) {
    for ($j = 1; $j <= $i; $j++) {
        echo "* ";
    }
    echo "\n";
}
```

**Output:**
```
* 
* * 
* * * 
* * * * 
* * * * * 
```

**Pattern 2: Inverted triangle.** Run the outer loop backwards, `for ($i = $n; $i >= 1; $i--)`, and keep the same inner loop. Row 1 then has 5 stars and the last row has 1.

**Pattern 3: Number triangle**

```php
<?php
// Example: Number triangle (print column number)
$n = 4;
for ($i = 1; $i <= $n; $i++) {
    for ($j = 1; $j <= $i; $j++) {
        echo $j . " ";
    }
    echo "\n";
}
```

**Output:**
```
1 
1 2 
1 2 3 
1 2 3 4 
```

**Pattern 4: Star pyramid**

For a pyramid of `n` rows, row `i` has `n - i` leading spaces and `2i - 1` stars.

| Row `i` (n = 5) | Spaces `n - i` | Stars `2i - 1` |
|---|---|---|
| 1 | 4 | 1 |
| 2 | 3 | 3 |
| 3 | 2 | 5 |
| 4 | 1 | 7 |
| 5 | 0 | 9 |

```php
<?php
// Example: Star pyramid (run in CLI or inside <pre>)
$n = 5;
for ($i = 1; $i <= $n; $i++) {
    for ($s = 1; $s <= $n - $i; $s++) {   // leading spaces
        echo " ";
    }
    for ($k = 1; $k <= 2 * $i - 1; $k++) { // stars
        echo "*";
    }
    echo "\n";
}
```

**Output:**
```
    *
   ***
  *****
 *******
*********
```

**Showing patterns in a browser.** A browser collapses multiple spaces and ignores `\n`. Either wrap the output in `<pre>...</pre>`, or use `&nbsp;` for spaces and `<br>` for new lines.

```php
<?php
// Example: Pyramid in a web page using <pre>
$n = 3;
echo "<pre>\n";
for ($i = 1; $i <= $n; $i++) {
    echo str_repeat(" ", $n - $i) . str_repeat("*", 2 * $i - 1) . "\n";
}
echo "</pre>\n";
```

**Output:**
```html
<pre>
  *
 ***
*****
</pre>
```

`str_repeat()` repeats a string a given number of times, which replaces the two inner loops. In the exam, write the nested-loop version unless the question allows built-in functions.

> **Teacher's Note:** For any pattern, have students first fill a small table: row number, number of spaces, number of symbols. Once the formula is in the table, the loops write themselves. This method works for every pattern question in the exam.

#### (h) Classic loop programs

**Multiplication table of a number**

```php
<?php
// Example: Multiplication table of 7
$num = 7;
for ($i = 1; $i <= 10; $i++) {
    echo "$num x $i = " . ($num * $i) . "\n";
}
```

**Output:**
```
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
```

**Multiplication table (1 to 5) as an HTML table, using nested loops**

```php
<?php
// Example: Multiplication grid as an HTML table
$size = 5;
echo "<table border=\"1\" cellpadding=\"4\">\n";

// header row
echo "  <tr><th>x</th>";
for ($c = 1; $c <= $size; $c++) {
    echo "<th>$c</th>";
}
echo "</tr>\n";

// body rows
for ($r = 1; $r <= $size; $r++) {
    echo "  <tr><th>$r</th>";
    for ($c = 1; $c <= $size; $c++) {
        echo "<td>" . ($r * $c) . "</td>";
    }
    echo "</tr>\n";
}
echo "</table>\n";
```

**Output:**
```html
<table border="1" cellpadding="4">
  <tr><th>x</th><th>1</th><th>2</th><th>3</th><th>4</th><th>5</th></tr>
  <tr><th>1</th><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
  <tr><th>2</th><td>2</td><td>4</td><td>6</td><td>8</td><td>10</td></tr>
  <tr><th>3</th><td>3</td><td>6</td><td>9</td><td>12</td><td>15</td></tr>
  <tr><th>4</th><td>4</td><td>8</td><td>12</td><td>16</td><td>20</td></tr>
  <tr><th>5</th><td>5</td><td>10</td><td>15</td><td>20</td><td>25</td></tr>
</table>
```

**Factorial of a number**

`n! = 1 × 2 × 3 × ... × n`, and `0! = 1`.

```php
<?php
// Example: Factorial using for loop
$n = 6;
$fact = 1;

if ($n < 0) {
    echo "Factorial is not defined for negative numbers.\n";
} else {
    for ($i = 2; $i <= $n; $i++) {
        $fact *= $i;
    }
    echo "$n! = $fact\n";
}
```

**Output:**
```
6! = 720
```

Note that the product starts at `1`, not `0`; starting at `0` would make every result `0`. Integers in 64-bit PHP overflow to float after `20!`.

**Prime number check**

A prime number is greater than 1 and divisible only by 1 and itself. We only need to test divisors up to the square root of `n`: if `n` had a divisor larger than `√n`, it would also have one smaller than `√n`.

```php
<?php
// Example: Check whether a number is prime
$n = 29;
$isPrime = true;

if ($n < 2) {
    $isPrime = false;
} else {
    for ($i = 2; $i * $i <= $n; $i++) {
        if ($n % $i === 0) {
            $isPrime = false;
            break;          // a divisor found; no need to test further
        }
    }
}

echo $isPrime ? "$n is a prime number.\n" : "$n is not a prime number.\n";
```

**Output:**
```
29 is a prime number.
```

**Prime numbers in a range (nested loops)**

```php
<?php
// Example: All prime numbers between 1 and 50
echo "Primes up to 50: ";
for ($n = 2; $n <= 50; $n++) {
    $isPrime = true;
    for ($i = 2; $i * $i <= $n; $i++) {
        if ($n % $i === 0) {
            $isPrime = false;
            break;
        }
    }
    if ($isPrime) {
        echo "$n ";
    }
}
echo "\n";
```

**Output:**
```
Primes up to 50: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 
```

**Fibonacci series**

Each term is the sum of the previous two: `0, 1, 1, 2, 3, 5, 8, ...`

```php
<?php
// Example: First 10 Fibonacci terms
$terms = 10;
$a = 0;
$b = 1;

echo "Fibonacci series: ";
for ($i = 1; $i <= $terms; $i++) {
    echo "$a ";
    $next = $a + $b;
    $a = $b;
    $b = $next;
}
echo "\n";
```

**Output:**
```
Fibonacci series: 0 1 1 2 3 5 8 13 21 34 
```

Trace of the variables:

| `$i` | printed `$a` | `$next` | new `$a` | new `$b` |
|---|---|---|---|---|
| 1 | 0 | 1 | 1 | 1 |
| 2 | 1 | 2 | 1 | 2 |
| 3 | 1 | 3 | 2 | 3 |
| 4 | 2 | 5 | 3 | 5 |
| 5 | 3 | 8 | 5 | 8 |

> **Exam Tip:** Programs on factorial, prime, Fibonacci, multiplication table, sum of digits, reverse/palindrome and star patterns are the most frequently asked "Write a PHP program" questions from this unit. Write `<?php` at the top, use meaningful variable names, add one or two comments, and show the expected output. Marks are given for logic even if a small syntax slip happens, so keep the logic clear.

### 2.2.2 Continue and Break

Normally a loop runs until its condition becomes false. Sometimes you need to change this from **inside** the body. PHP provides two **jump statements**:

| Statement | Effect |
|---|---|
| `break` | Exits the loop **immediately**. Control goes to the first statement after the loop. |
| `continue` | Skips the **rest of the current iteration** and moves to the next iteration (to the condition check in `while`, or the update in `for`). |

```
   break                                continue
   -----                                --------
   loop {                               loop {   <-------------+
       stmt A                               stmt A             |
       if (x) break; ---+                   if (x) continue; --+
       stmt B           |                   stmt B
   }                    |               }
   after loop  <--------+               after loop
```

#### `break`

You have already seen `break` inside `switch`. Inside a loop it stops the loop completely.

```php
<?php
// Example: break - stop searching once the student is found
$rollNumbers = [101, 104, 108, 112, 115, 120];
$search = 112;
$found  = false;

for ($i = 0; $i < count($rollNumbers); $i++) {
    echo "Checking position $i ...\n";
    if ($rollNumbers[$i] === $search) {
        $found = true;
        break;                  // exit the loop immediately
    }
}

echo $found ? "Roll $search found at position $i.\n" : "Roll $search not found.\n";
```

**Output:**
```
Checking position 0 ...
Checking position 1 ...
Checking position 2 ...
Checking position 3 ...
Roll 112 found at position 3.
```

Without `break`, the loop would keep checking positions 4 and 5 for no reason.

`break` is also the standard way to leave a deliberately endless loop such as `while (true) { ... if ($balance < 0) break; ... }`.

#### `continue`

```php
<?php
// Example: continue - print odd numbers only
for ($i = 1; $i <= 10; $i++) {
    if ($i % 2 === 0) {
        continue;               // skip even numbers
    }
    echo "$i ";
}
echo "\n";
```

**Output:**
```
1 3 5 7 9 
```

```php
<?php
// Example: continue - skip absent students when calculating the average
$marks = [78, -1, 65, 90, -1, 55];   // -1 means absent
$total = 0;
$count = 0;

for ($i = 0; $i < count($marks); $i++) {
    if ($marks[$i] < 0) {
        continue;               // ignore absent entries
    }
    $total += $marks[$i];
    $count++;
}

echo "Students present: $count\n";
echo "Average: " . round($total / $count, 2) . "\n";
```

**Output:**
```
Students present: 4
Average: 72
```

#### `break` vs `continue`

| Point | `break` | `continue` |
|---|---|---|
| Effect on the loop | Ends the loop | Ends only the current iteration |
| Next statement executed | First statement after the loop | Next iteration (condition / update) |
| Used in `switch` | Yes, to end a case | Acts like `break` inside a switch, but avoid it (PHP 7.3+ shows a warning) |
| Typical use | Stop when the answer is found, or on an error | Skip invalid or unwanted items |
| Example | Stop searching once a roll number is found | Skip even numbers, skip absent students |

> **Common Mistake:** Using `continue` in a `while` loop **before** the update.
> ```php
> $i = 0;
> while ($i < 10) {
>     if ($i == 5) continue;   // $i stays 5 forever -> infinite loop
>     echo $i;
>     $i++;
> }
> ```
> **Correct:** update first, then `continue`, or use a `for` loop, where the update in the header always runs after `continue`.
> ```php
> $i = 0;
> while ($i < 10) {
>     $i++;
>     if ($i == 5) continue;
>     echo $i;   // prints 1234678910
> }
> ```

#### Numeric argument: `break n` and `continue n`

In nested loops, a plain `break` or `continue` affects only the **innermost** loop. PHP lets you give a number to say **how many enclosing loops** to affect. `break 1` is the same as `break`.

- `break 2;` exits the inner loop **and** the outer loop.
- `continue 2;` skips the rest of the inner loop and goes to the **next iteration of the outer loop**.

The number must be a literal positive integer (not a variable) and cannot be larger than the number of nested loops.

```php
<?php
// Example: break vs break 2 in nested loops
echo "Using break (inner only):\n";
for ($i = 1; $i <= 3; $i++) {
    for ($j = 1; $j <= 3; $j++) {
        if ($j === 2) {
            break;              // leaves only the inner loop
        }
        echo "  i=$i j=$j\n";
    }
}

echo "Using break 2 (both loops):\n";
for ($i = 1; $i <= 3; $i++) {
    for ($j = 1; $j <= 3; $j++) {
        if ($i === 2 && $j === 2) {
            break 2;            // leaves inner AND outer loop
        }
        echo "  i=$i j=$j\n";
    }
}
echo "After loops\n";
```

**Output:**
```
Using break (inner only):
  i=1 j=1
  i=2 j=1
  i=3 j=1
Using break 2 (both loops):
  i=1 j=1
  i=1 j=2
  i=1 j=3
  i=2 j=1
After loops
```

**Real-world use of `break 2`:** find the first pair of items whose prices add up to a budget, then stop everything.

```php
<?php
// Example: Find two items that cost exactly Rs. 1500 together
$items  = ["Pen" => 50, "Bag" => 1200, "Book" => 450, "Shoes" => 1050, "Bottle" => 300];
$names  = array_keys($items);
$budget = 1500;
$result = "No matching pair found.";

for ($i = 0; $i < count($names); $i++) {
    for ($j = $i + 1; $j < count($names); $j++) {
        if ($items[$names[$i]] + $items[$names[$j]] === $budget) {
            $result = "Buy {$names[$i]} and {$names[$j]}.";
            break 2;            // pair found, stop both loops
        }
    }
}

echo $result . "\n";
```

**Output:**
```
Buy Bag and Bottle.
```

(Bag 1200 + Book 450 = 1650 and Bag + Shoes = 2250 are checked first; Bag + Bottle = 1500 is the first match.)

```php
<?php
// Example: continue 2 - list students, skip anyone with a failed subject
$students = [
    "Aarav"  => [65, 72, 80],
    "Binita" => [55, 30, 70],
    "Chirag" => [88, 91, 79],
];

foreach ($students as $name => $marks) {   // foreach: see Unit 3
    for ($k = 0; $k < count($marks); $k++) {
        if ($marks[$k] < 40) {
            echo "$name: has a failed subject, skipped\n";
            continue 2;         // jump to the next student (outer loop)
        }
    }
    echo "$name: passed all subjects, total " . array_sum($marks) . "\n";
}
```

**Output:**
```
Aarav: passed all subjects, total 217
Binita: has a failed subject, skipped
Chirag: passed all subjects, total 258
```

> **Teacher's Note:** Act out `break 2` with two volunteers: one walks the "rows" (outer loop), the other counts the "columns" (inner loop). When the inner person shouts "break 2", both stop. With a plain "break", only the inner person stops and the outer person moves to the next row. About 10 minutes, and students remember it.

> **Exam Tip:** For "Explain break and continue with example", give one short example of each, show their outputs side by side, and add the comparison table. Mentioning `break 2` for nested loops shows deeper understanding and often earns the full marks.

---

## Summary

- **Control structures** change the normal top-to-bottom flow. There are three building blocks: sequence, selection and iteration.
- `if` runs a block only when a condition is true; `if...else` chooses between two blocks; `if...elseif...else` checks conditions in order and runs the **first** true one.
- A **nested if** is an `if` inside another `if`; flatten deep nesting with `&&` / `||` where possible.
- The **alternative syntax** (`if (...):`, `elseif:`, `else:`, `endif;`, also `endwhile;`, `endfor;`, `endswitch;`, `endforeach;`) makes PHP inside HTML templates easier to read. `do...while` has no alternative syntax.
- The **ternary operator** `condition ? a : b` is a one-line `if...else` that returns a value. Nested ternaries need parentheses in PHP 8.
- `$a ?: $b` returns `$b` when `$a` is falsy; `$a ?? $b` returns `$b` when `$a` is `null` or not set, with no warning. Use `??` to read `$_GET` / `$_POST` with defaults.
- `switch` compares one expression with many case values using loose `==`. `break` ends a case; without it execution **falls through** to the next case. `default` runs when nothing matches.
- Group cases by stacking them with no statements in between. PHP 8 `match` is a strict, value-returning alternative (beyond the syllabus).
- A **loop** repeats a block; its four parts are initialization, condition, body and update. A missing update causes an **infinite loop**.
- `while` and `for` are **entry-controlled** (may run 0 times); `do...while` is **exit-controlled** (runs at least once) and ends with a semicolon.
- Use `for` when the number of repetitions is known, `while` when it is unknown, `do...while` when the body must run at least once, and `foreach` (Unit 3) for arrays.
- In a **nested loop** the inner loop completes fully for each outer iteration; outer loop = rows, inner loop = columns in pattern programs.
- Loops are used to generate HTML (table rows, `<option>` lists). Use `<pre>` or `&nbsp;`/`<br>` to show patterns in a browser.
- `break` exits a loop; `continue` skips to the next iteration. `break 2` / `continue 2` act on two levels of nested loops.

---

## Practice Questions

### Short Answer Questions (2-3 marks)

1. What is a control structure? Name the three basic types with one PHP example of each.
2. Write the syntax of the `if...elseif...else` statement and explain how PHP decides which block to run.
3. What is a nested `if`? Give a short example.
4. What is the alternative syntax of control structures in PHP? Why is it useful in HTML templates?
5. What is the ternary operator? Rewrite `if ($age >= 18) { $s = "Adult"; } else { $s = "Minor"; }` using it.
6. Differentiate between `?:` and `??` with an example of each.
7. What is fall-through in a `switch` statement? When is it useful?
8. What is an infinite loop? Give two reasons it can occur.
9. Differentiate between entry-controlled and exit-controlled loops.
10. What is the output of the following code? Explain.
    ```php
    <?php
    for ($i = 1; $i <= 10; $i++) {
        if ($i % 3 === 0) continue;
        if ($i === 8) break;
        echo $i . " ";
    }
    ```
    (Answer: `1 2 4 5 7`. Multiples of 3 are skipped, and the loop ends when `$i` becomes 8.)

### Long Answer Questions (5-10 marks)

1. Explain the different forms of conditional statements in PHP (`if`, `if...else`, `if...elseif...else`, nested `if`) with syntax, flowchart and example.
2. Explain the `switch` statement with syntax and flowchart. What is the role of `break` and `default`? Write a PHP program that displays the name of the month for a month number (1 to 12) using `switch`.
3. Differentiate between `if...elseif` and `switch`. Write a PHP program that takes marks of a student and displays the grade using `if...elseif...else`.
4. Explain `while`, `do...while` and `for` loops with syntax and flowchart. Differentiate between `while` and `do...while` with a suitable example.
5. Explain `break` and `continue` statements with examples. How do `break 2` and `continue 2` work in nested loops?
6. Write a PHP program to:
   (a) display the multiplication table of a number in an HTML table, and
   (b) print the following pattern using nested loops:
   ```
       *
      ***
     *****
    *******
   ```

### Programming Exercises

1. **(Easy)** Write a PHP program to check whether a number is positive, negative or zero.
2. **(Easy)** Write a PHP program to print all numbers from 1 to 100 that are divisible by both 3 and 5, using a `for` loop.
3. **(Easy)** Using a `while` loop, find the sum of digits of 98765 and count how many digits it has.
4. **(Medium)** Write a PHP program that uses `switch` to display the type of a vehicle based on a code: `B` = Bus, `T` = Truck, `C` = Car, `M` = Motorbike, anything else = "Unknown". Accept both upper and lower case.
5. **(Medium)** Write a PHP program to check whether a number is an Armstrong number (e.g., 153 = 1³ + 5³ + 3³).
6. **(Medium)** Print the first 15 Fibonacci numbers in an HTML `<ul>` list, highlighting even numbers with `<strong>` using the ternary operator.
7. **(Hard)** Generate an HTML table of all prime numbers between 1 and 100, 10 numbers per row.
8. **(Hard)** Print the following number pyramid using nested loops:
   ```
      1
     121
    12321
   1234321
   ```

---

## Lab Work

### Lab 2.1: Marksheet Result Page with Conditionals

**Objective:** Use `if...elseif...else`, the ternary operator, `??` and the alternative syntax to build a small result page from form input.

**Steps:**

1. Create `result.php` in your `htdocs` folder (XAMPP).
2. Add an HTML form (method `get`) with fields: student name and marks for three subjects.
3. Read the values with `filter_input()` and `??`. Validate that each mark is an integer from 0 to 100.
4. Calculate the total and percentage. Assign a division with `if...elseif...else`: 80+ Distinction, 60+ First, 45+ Second, 32+ Third (only if every subject is 32 or more), otherwise Fail.
5. Display the result in an HTML table using the alternative syntax. Use a ternary to add a CSS class `pass` or `fail` to each subject mark.
6. Escape the student name with `htmlspecialchars()` before printing.

**Expected outcome:** Submitting `Sita`, `78`, `85`, `69` shows Total 232, Percentage 77.33, Division "First Division", and all marks in green. An invalid mark such as `120` shows an error message instead of the result.

**Reference solution:**

```php
<?php
// result.php - Lab 2.1 reference solution
$name = trim(filter_input(INPUT_GET, 'name') ?? '');
$opts = ['options' => ['min_range' => 0, 'max_range' => 100]];
$subjects = ['English', 'Maths', 'Computer'];
$marks = [];
$error = '';

if (isset($_GET['name'])) {                       // form submitted
    for ($i = 0; $i < 3; $i++) {
        $m = filter_input(INPUT_GET, "m$i", FILTER_VALIDATE_INT, $opts);
        if ($m === false || $m === null) {
            $error = "Marks for {$subjects[$i]} must be a whole number from 0 to 100.";
            break;
        }
        $marks[] = $m;
    }
    if ($name === '') {
        $error = "Please enter the student name.";
    }
}

if (count($marks) === 3 && $error === '') {
    $total   = array_sum($marks);
    $percent = round($total / 3, 2);
    $allPass = min($marks) >= 32;

    if (!$allPass)          { $division = "Fail"; }
    elseif ($percent >= 80) { $division = "Distinction"; }
    elseif ($percent >= 60) { $division = "First Division"; }
    elseif ($percent >= 45) { $division = "Second Division"; }
    else                    { $division = "Third Division"; }
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Result</title>
    <style>.pass{color:green}.fail{color:red}td,th{padding:4px 10px}</style>
</head>
<body>
<form method="get">
    Name: <input name="name" value="<?= htmlspecialchars($name) ?>"><br>
    <?php for ($i = 0; $i < 3; $i++): ?>
        <?= $subjects[$i] ?>: <input name="m<?= $i ?>" type="number" min="0" max="100"><br>
    <?php endfor; ?>
    <button>Show Result</button>
</form>

<?php if ($error !== ''): ?>
    <p class="fail"><?= htmlspecialchars($error) ?></p>
<?php elseif (isset($division)): ?>
    <h2>Result of <?= htmlspecialchars($name) ?></h2>
    <table border="1">
        <tr><th>Subject</th><th>Marks</th></tr>
        <?php for ($i = 0; $i < 3; $i++): ?>
            <tr>
                <td><?= $subjects[$i] ?></td>
                <td class="<?= $marks[$i] >= 32 ? 'pass' : 'fail' ?>"><?= $marks[$i] ?></td>
            </tr>
        <?php endfor; ?>
        <tr><th>Total</th><th><?= $total ?></th></tr>
        <tr><th>Percentage</th><th><?= $percent ?>%</th></tr>
        <tr><th>Division</th><th><?= $division ?></th></tr>
    </table>
<?php endif; ?>
</body>
</html>
```

### Lab 2.2: Loop Workshop Page

**Objective:** Practise `for`, `while`, `do...while`, nested loops, `break` and `continue` by generating HTML output.

**Steps:**

1. Create `loops.php`. Read a number `n` from the URL (`loops.php?n=7`) with `filter_input(..., FILTER_VALIDATE_INT)` and default to 5 using `??`. Limit it to the range 1 to 20.
2. Section 1: print the multiplication table of `n` in an HTML table using `for`.
3. Section 2: print `n!` using `while`, and the first `n` Fibonacci terms using `for`.
4. Section 3: print a star pyramid of `n` rows inside `<pre>` using nested loops.
5. Section 4: print all primes up to `10 * n`, using `break` in the inner loop as soon as a divisor is found.
6. Section 5: print numbers from 1 to `5 * n`, skipping multiples of 4 with `continue`.
7. Section 6: use a `do...while` loop to roll a die (`random_int(1, 6)`) until a 6 appears, listing each roll.

**Expected outcome:** For `n = 5`, the page shows the table of 5 (5 to 50), `5! = 120`, Fibonacci `0 1 1 2 3`, a 5-row pyramid, primes up to 50 (`2 3 5 7 ... 47`), the numbers 1 to 25 without 4, 8, 12, 16, 20 and 24, and a list of die rolls that always ends with 6. Viewing the page source shows only HTML, no PHP code.
