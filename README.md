# Homework 2

* Assigned: 9/29
* Due: 10/7 11:59PM (NO GRACE DAYS!!)
* Worth 3.75% of your grade
* Done and submitted individually (as with all the homeworks) **via [Gradescope](https://www.gradescope.com)**


## Submission

1. You will write your answers using the [following Overleaf latex template](https://www.overleaf.com/read/mvhfmrqpnqbs). Clone the template and edit it.   Hand writing or alternative submission formats are NOT accecptable.
2. You will then submit the compiled PDF on GradeScope


## 1. Relational Algebra

**(2 points each, 6 points total)**

Consider the simplified Employee Stock Ownership Database below (primary keys are in **bold**):

* Person(**ssn**, companyid, salary, managerid)

This is an employee table. *companyid* is a foreign key which points to *Company*.
We assume one employee can only work at one company. *managerid* is a foreign key
which points to another *Person* record .

* Company(**companyid**, companyname, location)

* Holding(**ssn**, **companyid**, sharenum)

Holding table describes an employee(*ssn*) owns *sharenum* stocks of *companyid*.

Construct relational algebra for the following queries:

* **Q1**: Find the *ssn* of the persons who work at Google(*companyid* = 601) and hold more than 500 *sharenum*
   of stock from Facebook(*companyid* = 700).

* **Q2**: Find the *ssn* of the persons whose stocks are all different from those his/her manager owns.
    (Note: Each *companyid* represents a different stock, we do not care about the *sharenum* of stocks).

* **Q3**: Find the *ssn* of the persons who own at least three different stocks.


## 2. More Relational Algebra

**(2 point each, 10 points total)**

T1

|A | B | C |
|---|---|---|
|1 | x | a |
|2 | y | c |
|2 | y | b |
|2 | z | c |


T2

B | C | D
---|---|---
1 | x | c
2 | y | c
3 | x | a


Write the result table for the relational algebra expressions in a similar with T1 and T2:

Note: Here, we assume the attributes whose values are all numbers are the same type: integers
and all the letters are the same type: char. If the result table is empty, write the schema of the table.


1. π<sub>A,B</sub>(T1)

2. π<sub>D</sub>(T2)

3. T1 × π<sub>A</sub>(T1)

4. T1 ⨝<sub>T1.B=T2.C</sub> T2

5. T1 − (T1 ∩ T2)

6. T1 ⨝<sub>T1.A&gt;T2.B</sub> (σ<sub>B&ne;2</sub>(T2))


## 3. SQL

**(2 points each, 6 points total)**

Here are three relationship, (primary keys are in **bold**):

* Store(**storeid**, s_name, employee_number, city)

* Goods(**g_id**, g_name, price)

* Supply(**storeid**, **g_id**)

### Requirements:

First, describe the meaning of following relational algebra expressions in one or two sentences.
Second, translate the following relational algebra expressions in SQL. Make sure your SQL can be executed.


1. π<sub>storeid, s_name</sub>(σ<sub>employee_number<=100 or city = "New York"</sub>(Store))

2. π<sub>s_name</sub>(((σ<sub>g_name = "pencil"</sub>Goods) ⨝ Supply) ⨝ Store)

3. π<sub>s_name, city</sub>((Supply/π<sub>g_id</sub>(σ<sub>storeid='0808'</sub>(Supply)))⨝ Store)
