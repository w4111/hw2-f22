# Homework 2 Solution

## 1.1 

π<sub>ssn</sub>(σ<sub>companyid=601</sub>Person) ∩ 
π<sub>ssn</sub>(σ<sub>companyid=700 ∧ sharenum > 500 </sub> (Holding)) 

Or there is another solution:

π<sub>ssn</sub>(σ<sub>companyid=601</sub>Person ⨝ σ<sub>companyid=700 ∧ sharenum > 500 </sub> (Holding)) 

## 1.2 

p(PersonStocks, π<sub>Person.ssn, Holding.companyid</sub> (Person ⨝<sub>Person.ssn = Holding.ssn</sub> Holding))

p(ManagerStocks, π<sub>Person.ssn, Holding.companyid</sub> (Person ⨝<sub>Person.managerid = Holding.ssn</sub> Holding))

π<sub>ssn</sub>(Person) - π<sub>ssn</sub>(PersonStocks ∩ ManagerStocks)


## 1.3

p(tmp1, Holding)

p(tmp2, Holding)

π <sub>ssn</sub>(σ<sub>Holding.companyid ≠ tmp1.companyid ∧Holding.companyid  ≠ tmp2.companyid ∧ tmp2.companyid≠ tmp1.companyid</sub>(Holding ⨝<sub>ssn</sub> tmp1 ⨝<sub>ssn</sub> tmp2))

## 2.1     

π<sub>A,B</sub>(T1)

|A | B |
|---|---|
|1 | x |
|2 | y |
|2 | z |

## 2.2

π<sub>D</sub>(T2)

|D |
|--|
|c |
|a |

## 2.3

T1 × π<sub>A</sub>(T1)

|(A) | B | C |  (A)|
|---|---|---| ---|
|1 | x | a | 1 |
|2 | y | c | 1 |
|2 | y | b | 1 |
|2 | z | c | 1 |
|1 | x | a | 2 |
|2 | y | c | 2 |
|2 | y | b | 2 |
|2 | z | c | 2 |

## 2.4

T1 ⨝<sub>T1.B=T2.C</sub> T2

|A | (B) | (C) | (B) | (C) | D |
|---|---|---|---|---|---|
|1 | x | a | 1 | x | c |
|1 | x | a | 3 | x | a |
|2 | y | c | 2 | y | c |
|2 | y | b | 2 | y | c |

## 2.5

T1 − (T1 ∩ T2)

|A | B | C |
|---|---|---|
|1 | x | a |
|2 | y | b |
|2 | z | c |

## 2.6

T1 ⨝<sub>T1.A&gt;T2.B</sub> (σ<sub>B&ne;2</sub>(T2))

|A | (B) | (C) | (B) | (C) | D |
|--|-----|---|-----|---|---|
|2 | y   | c | 1   | x | c |
|2 | y   | b | 1   | x | c |
|2 | z   | c | 1   | x | c |

IMPORTANT: You should use the bracket `(<column_name>)` notation or the `<table_name>.<column_name>` *only* for columns with duplicate names! 

In other words, if you bracket every column, or if you put a `<table_name>.` prefix in front of every column, then it's incorrect.

## 3.1 

description: Find the id and name of the stores which have <= 100 employees or located in New York.

```sql
select storeid, s_name from Store 
where employee_numer <= 100 or city = "New York"
```

## 3.2 

description: Find the name of the stores which supply pencils.

```sql
select dinstinct s_name 
from Store, Goods, Supply
where Goods.g_id = "pencil" 
    and Goods.g_id = Supply.g_id
    and Store.storeid = Supply.storeid
```



## 3.3

description: Find the name and city of the stores which supply all the goods that store "0808" has.

```sql
select distinct s_name, city
from Store
where not exists(
    select * from Supply as S1
    where S1.storeid = "0808" 
    and not exists(
        select * from Supply as S2 
        where S2.storeid = Store.storeid and 
        S2.g_id = S1.g_id
    )
)
```

* Store(**storeid**, s_name, employee_number, city)

* Goods(**g_id**, g_name, price)

* Supply(**storeid**, **g_id**)

## 4.1
Find stores in the city "NYC" that supply goods named "Pokemon". Return `storeid` and the associated `s_name`.
```sql
select Store.storeid, Store.s_name
from Store, Supply, Goods
where Store.s_name = 'NYC' and 
      Goods.g_name = 'Pokemon' and 
      Store.storeid = Supply.storeid and
      Supply.g_id = Goods.g_id
```


## 4.2
Find stores that supply at least two types of goods: those named "Pokemon" and those named "Digimon." Return storeid and the associated s_name.

```sql
select Store.storeid, Store.s_name
from Store, Supply S1, Supply S2, Goods G1, Goods G2
where G1.g_name = 'Pokemon' and G2.g_name = 'Digimon' and
      S1.g_id = G1.g_id and S2.g_id = G2.g_id and 
      Store.storeid = S1.storeid and Store.storeid = S2.storeid
```

We can also use intersection.
```sql
select Store.storeid, Store.s_name
from Store, Supply, Goods
where Goods.g_id = 'Pokemon'
      Store.storeid = Supply.storeid and Supply.g_id = Goods.g_id

intersect

select Store.storeid, Store.s_name
from Store, Supply, Goods
where Goods.g_id = 'Digimon'
      Store.storeid = Supply.storeid and Supply.g_id = Goods.g_id
```

## 4.3
How many stores in the city "NYC" have at least 10 employees (as specified by employee_number) and supply goods that cost less than 10 dollars (as specified by price)? Return a number.

```sql
select count(distinct storeid)
from Store, Goods, Supply
where Store.employee_number >= 10 and price < 10 and 
      Store.storeid = Supply.storeid and
      Supply.g_id = Goods.g_id

```

## 4.4
Return the number of goods that are supplied by every store in the city "Springfield". Return a number.

```sql
select count(distinct Goods.gid)
from Store, Supply, Goods
where Store.s_name = "Springfield" and
      Store.storeid = Supply.storeid and 
      Supply.g_id = Goods.g_id
```