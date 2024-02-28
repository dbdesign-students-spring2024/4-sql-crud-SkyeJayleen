# SQL Crud Workshop

## Part 1: Restaurant Finder

### Creating & Displaying the Table

```
CREATE TABLE restaurants(id integer primary key, name text, category text, pricetier text, neighborhood text, openinghr text, closinghr text, ratingstars integer, kidfriendly integer);

.import restaurants.csv restaurants --skip 1

.mode markdown

.headers on

select * from restaurants limit 10;
```

### Queries

1. Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).

```
select name from restaurants where neighborhood = "chelsea" and pricetier = "cheap";
```

2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.

```
select name, ratingstars from restaurants where category = "korean" and ratingstars > 2 order by ratingstars desc;
```

3. Find all restaurants that are open now (see hint below).

```
select name from restaurants where openinghr <= strftime('%H:%M', 'now', 'localtime') and closinghr >= strftime('%H:%M', 'now', 'localtime');
```

4. Leave a review for a restaurant (pick any restaurant as an example; note that leaving a review has no automatic effect on the average rating of the restaurant).

```

```

5. Delete all restaurants that are not good for kids.

```

```

6. Find the number of restaurants in each NYC neighborhood.

```

```


## Part 2: Social Media App

### Creating & Displaying the Table

```
.mode csv

CREATE table users(id integer primary key, handle text, email text, password text);
.import users.csv users --skip 1

CREATE TABLE posts(id integer primary key, message text, sender text, receiver text, story text, createdate text, createtime text);
.import posts.csv posts --skip 1

.mode markdown
.header on
select * from users limit 10;
select * from posts limit 10;
```

### Queries

1. Register a new User.

```
insert into users (handle, email, password) values ("SkyeJayleen", "jl11695@nyu.edu", "12345")
```

2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).

```
insert into posts (message, sender, receiver) values ("hey there!", "1", "2");
```

3. Create a new Story by a particular User (pick any User for example).

```
insert into posts (sender, story, createdate, createtime) values ("3", "this is my story.", "2024-02-27", "17:42:30");
```

4. Show the 10 most recent visible Messages and Stories, in order of recency.

```
select message, story from posts where ROUND((JULIANDAY('now') - JULIANDAY(posts.createdate)) * 24) < 24;
```

# Need this to incorporate create time.


5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.

6. Make all Stories that are more than 24 hours old invisible.

7. Show all invisible Messages and Stories, in order of recency.

8. Show the number of posts by each User.

9. Show the post text and email address of all posts and the User who made them within the last 24 hours.

10. Show the email addresses of all Users who have not posted anything yet.