## Using String as Primary Key

reference: https://stackoverflow.com/questions/3455297/mysql-using-string-as-primary-key

> There's nothing wrong with using a CHAR or VARCHAR as a primary key.
>
> Sure it'll take up a little more space than an INT in many cases, but there are many cases where it is the most logical choice and may even reduce the number of columns you need, improving efficiency, by avoiding the need to have a separate ID field.


## What is the difference between a primary key and a index key

reference: https://stackoverflow.com/questions/5374908/what-is-the-difference-between-a-primary-key-and-a-index-key

> A primary key is a special kind of index in that:
>
> - there can be only one;
> - it cannot be nullable; and
> - it must be unique.