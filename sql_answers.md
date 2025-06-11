


## Answers

1. `select * from Regions;`
1. `select count(*) from Orders;` = 830
1. `select CategoryName, Description from Categories order by CategoryName asc;`
1. `select CompanyName, ContactName from Customers where City = 'Buenos Aires';` (double quotes will work in SQLite, ie `"Buenos Aires"`, but use single quotes to get into the habit for PostgreSQL)
1. `select count(*) from Customers where Country not in ('Germany', 'Mexico', 'Spain');` = 70