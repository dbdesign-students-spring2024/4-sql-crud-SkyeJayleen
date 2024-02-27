# SQL Crud Workshop

## Part 1: Restaurant Finder

### Creating the table

```
CREATE TABLE restaurants(id integer primary key, category text, pricetier text, neighborhood text, openinghr text, ratingstars integer, kidfriendly integer)

.mode csv
.headers on
select * from restaurants;
.import restaurants.csv restaurants --skip 1
```

