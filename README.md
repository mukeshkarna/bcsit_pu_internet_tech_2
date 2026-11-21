# Internet Technology II (CMP 273): Course Notes

**Pokhara University · Faculty of Management Studies · BCSIT, Semester III**
**Nature:** Theory + Practical · **Credit hours:** 3 · **Total periods:** 48 hours · **Full marks:** 100 (Pass: 45)

These notes follow the official syllabus unit by unit. They are written so that the **teacher** can teach from them and **students** can use them for self-study and exam preparation.

## How to use these notes

| Box | Who it is for | Meaning |
|-----|---------------|---------|
| **Teacher's Note** | Teacher (students can read too) | Teaching tip, demo idea, analogy |
| **Common Mistake** | Students | An error students often make, and the correct version |
| **Exam Tip** | Students | What is often asked and how to write the answer |

Each unit ends with a **Summary** for quick revision, **Practice Questions** (short answer, long answer, programming), and **Lab Work** linked to the syllabus lab list.

All code targets **PHP 8.x** and runs on **XAMPP** (Apache + PHP + MySQL/MariaDB). Save files in `htdocs/` and open them at `http://localhost/...`, or run `php -S localhost:8000` inside a project folder.

## Units

| Unit | Title | Hours | Notes |
|------|-------|-------|-------|
| 1 | Introduction | 4 | [Unit-1-Introduction.md](Unit-1-Introduction.md) |
| 2 | Control Structures and Loop | 5 | [Unit-2-Control-Structures-and-Loop.md](Unit-2-Control-Structures-and-Loop.md) |
| 3 | Array and Function | 7 | [Unit-3-Array-and-Function.md](Unit-3-Array-and-Function.md) |
| 4 | Form Handling and Data Validation | 6 | [Unit-4-Form-Handling-and-Data-Validation.md](Unit-4-Form-Handling-and-Data-Validation.md) |
| 5 | File Handling, Sessions, and Error Handling | 6 | [Unit-5-File-Handling-Sessions-and-Error-Handling.md](Unit-5-File-Handling-Sessions-and-Error-Handling.md) |
| 6 | Working with Database | 8 | [Unit-6-Working-with-Database.md](Unit-6-Working-with-Database.md) |
| 7 | Advanced PHP Concepts (OOP) | 6 | [Unit-7-Advanced-PHP-Concepts.md](Unit-7-Advanced-PHP-Concepts.md) |
| 8 | PHP Framework (MVC, Laravel) | 6 | [Unit-8-PHP-Framework.md](Unit-8-PHP-Framework.md) |

## Syllabus lab work and where it is covered

| # | Lab exercise (from syllabus) | Covered in |
|---|------------------------------|------------|
| 1 | Basic PHP web page that displays dynamic content and interacts with the user | Units 1–3 |
| 2 | HTML form with various input elements and server-side validation | Unit 4 |
| 3 | Form to upload images using PHP | Unit 4 (4.6) |
| 4 | Session-based authentication and authorization | Unit 5 (5.5), Unit 6 (6.5) |
| 5 | CRUD operations in MySQL database using PHP | Unit 6 (6.3) |
| 6 | General concept of Laravel, Symfony, or CodeIgniter | Unit 8 |

> **Note (from syllabus):** Students should build a small project that combines all of the above. Each lab session should have more than 4 hours of practical work.

### Suggested mini-project ideas
- Student Management System (login, add/edit/delete students, upload photo, search)
- Library Book Issue System
- Online Notice Board for a college (admin posts notices, students view)
- Simple Online Shop with cart (sessions) and orders (transactions)

## Evaluation scheme

| External | Marks | Internal (Theory, 30 marks) | Weight | Internal (Practical, 20 marks) | Weight |
|----------|-------|-----------------------------|--------|--------------------------------|--------|
| Semester-end exam | 50 | Attendance & participation | 10% | Attendance & participation | 10% |
| | | Assignments | 20% | Lab / project report | 20% |
| | | Presentations / quizzes | 10% | Practical exam / project work | 40% |
| | | Internal assessment | 60% | Viva | 30% |

Students need at least **45%** separately in internal assessment and practical, plus **80% attendance**, to sit the semester-end exam.

## Books

**Prescribed text:** Larry Ullman (2018). *PHP and MySQL for Dynamic Web Sites: Visual QuickPro Guide* (5th ed.). Peachpit Press.

**References:**
1. Butler, T., & Yank, K. (2017). *PHP & MySQL: Novice to Ninja* (6th ed.). SitePoint.
2. Lockhart, J. (2015). *Modern PHP: New Features and Good Practices*. O'Reilly.
3. Nixon, R. (2015). *Learning PHP, MySQL & JavaScript* (5th ed.). O'Reilly.

**Online:** PHP manual (https://www.php.net/manual/en/), Laravel docs (https://laravel.com/docs).
