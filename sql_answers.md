


## Answers

1. `select * from Regions;`
1. `select count(*) from Orders;` = 830
1. `select CategoryName, Description from Categories order by CategoryName asc;`
1. `select CompanyName, ContactName from Customers where City = 'Buenos Aires';` (double quotes will work in SQLite, ie `"Buenos Aires"`, but use single quotes to get into the habit for PostgreSQL)
1. `select count(*) from Customers where Country not in ('Germany', 'Mexico', 'Spain') or Country is null;` = 70
1. `select distinct Country from Customers order by Country asc;`
1. `select distinct City from Customers where City like 'A%' or City like 'C%';`