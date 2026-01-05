# catechism

- download and install [SQLite](https://www.sqlite.org/)
- download `northwind.db` (included here in this repo)
- open the database using a command like `sqlite3 ~/Downloads/northwind.db` (this will put you in a shell that is connected to the database file)
- type `.tables` to see the available tables
- run the query `select * from Shippers;`
- type `.mode column`
- run the query again (note the difference)
- do the questions and answers

## Questions

1. Get all columns and rows from the `Regions` table
1. How many rows are in the `Orders` table?
1. From the `Categories` table, get the `CategoryName` and `Description` of all rows, sorted by `CategoryName` in ascending alphabetical order
1. From the `Customers` table, get the `CompanyName` and `ContactName` of everyone in the `City` of `Buenos Aires`
1. How many `Customers` have a `Country` that isn't `Germany`, `Spain`, or `Mexico`? A `Country` of `null` is presumed to not be one of those countries.
1. From the `Customers` table, return all the countries present (`Country`) in alphabetical order (*with no duplicates*)
1. From the `Customers` table, list all cities (`City`) starting with the letter `A` or `C`. Don't include duplicates.
1. In the `Orders` table, one `CustomerID` may be associated with many `OrderID`s. Get the count of orders per `CustomerID`, in ascending alphabetical order of `CustomerID`. What's the first result?
1. There are 77 products in the `Products` table, and 29 suppliers in the `Suppliers` table. Using a `left join`, list the name of all products (and the name of their supplier) where the `CompanyName` of the supplier is one of `Exotic Liquids`, `Specialty Biscuits, Ltd`, `Escargots Nouveaux`.

## Answers

1. `select * from Regions;`
    - this is the most basic SQL query which you will use all the time, `*` means 'all columns', and since we're not restricting the rows in any way, we get all the rows
1. `select count(*) from Orders;`
    - answer: `830`
1.  `select CategoryName, Description from Categories order by CategoryName asc;`

    You can do on one line, but the formatting below is better:

    ```sql
    select
        CategoryName,
        Description
    from
        Categories
    order by
        CategoryName asc;
    ```

1. `select CompanyName, ContactName from Customers where City = 'Buenos Aires';`
    - double quotes will work in SQLite, ie `"Buenos Aires"`, but use single quotes to get into the habit for PostgreSQL
1. `select count(*) from Customers where Country not in ('Germany', 'Mexico', 'Spain') or Country is null;`
    - answer: `72`
    - half marks if you didn't include `or Country is null`. When you think of `null`, think "unknown". Is an unknown country not in Germany, Mexico, Spain? SQL doesn't know, so will conservatively just give you `70`. We have to explicitly deal with the `null` case.
1. `select distinct Country from Customers order by Country asc;`
    - note, `distinct` works by sorting the results behind the scenes, so be conscious of that when working with large data (because it will have to sort the entire table every single time)
1. `select distinct City from Customers where City like 'A%' or City like 'C%';`
    - sqlite unfortunately doesn't have `ilike` which is case-insensitive (not that it matters in this case)
    - `like '%blah%` is a very common pattern to see if some particular text is present *anywhere* in a string
1. `select CustomerID, count(OrderID) from Orders group by CustomerID order by CustomerID asc;`
    - answer: `ALFKI 6`
1. 🐧
    - Note, normally a table's own ID is just `id`, so `left join Suppliers on Suppliers.ID = Products.SupplierID` would be more typical, but this database isn't named like that.
    - Also, I like to be very explicit every time with which table a column came from, so `select Products.ProductName, Suppliers.CompanyName ...` instead of `select ProductName, CompanyName ...`, even though the 2nd one will work too in this case because those no ambiguity about which table they came from.

    ```sql
    select
        Products.ProductName,
        Suppliers.CompanyName
    from Products
    left join Suppliers on Suppliers.SupplierID = Products.SupplierID
    where Suppliers.CompanyName in ('Exotic Liquids', 'Specialty Biscuits, Ltd.', 'Escargots Nouveaux');
    ```

    Output, order doesn't matter

    ```
    ProductName                 CompanyName
    --------------------------  ------------------------
    Chai                        Exotic Liquids
    Chang                       Exotic Liquids
    Aniseed Syrup               Exotic Liquids
    Teatime Chocolate Biscuits  Specialty Biscuits, Ltd.
    Sir Rodney's Marmalade      Specialty Biscuits, Ltd.
    Sir Rodney's Scones         Specialty Biscuits, Ltd.
    Escargots de Bourgogne      Escargots Nouveaux
    Scottish Longbreads         Specialty Biscuits, Ltd.
    ```
