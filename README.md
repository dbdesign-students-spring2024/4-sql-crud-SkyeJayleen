# SQL Crud Workshop

## Part 1: Restaurant Finder

Link to files:
- [restaurants.csv](data/restaurants.csv)
- [reviews.csv](data/reviews.csv)

### Creating The Table And Importing Data

```
.mode csv
.headers on

CREATE TABLE restaurants(id integer primary key, name text, category text, pricetier text, neighborhood text, openinghr text, closinghr text, ratingstars integer, kidfriendly integer);

.import restaurants.csv restaurants --skip 1

CREATE TABLE reviews(id integer primary key, restaurant_id integer, review text);

.import reviews.csv reviews --skip 1
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
insert into reviews (restaurant_id, review) values ("1", "Great fusion of Japanese and Chinese flavors.");
```

5. Delete all restaurants that are not good for kids.

```
delete from restaurants where kidfriendly = "false";
```

6. Find the number of restaurants in each NYC neighborhood.

```
select neighborhood, count(id) as num_restaurants from restaurants group by neighborhood;
```


## Part 2: Social Media App

Link to files:
- [users.csv](data/users.csv)
- [posts.csv](data/posts.csv)

### Creating The Table And Importing Data

```
.mode csv
.headers on

CREATE table users(id integer primary key, handle text, email text, password text);

.import users.csv users --skip 1

CREATE TABLE posts(id integer primary key, message text, sender text, receiver text, story text, createdate text, createtime text, createdatetime text, visibility integer);

.import posts.csv posts --skip 1
```

### Queries

1. Register a new User.

```
insert into users (handle, email, password) values ("SkyeJayleen", "jl11695@nyu.edu", "12345");
```

2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).

```
insert into posts (message, sender, receiver, createdatetime, visibility) values ("hello!", "2", "1", datetime('now', 'localtime'), "TRUE");
```

3. Create a new Story by a particular User (pick any User for example).

```
insert into posts (sender, story, createdatetime, visibility) values ("3", "this is my story.", datetime('now', 'localtime'), "TRUE");
```

4. Show the 10 most recent visible Messages and Stories, in order of recency.

```
select message, story from posts where ROUND((JULIANDAY('now') - JULIANDAY(posts.createdatetime)) * 24) < 24 and ((message is not "") or (story is not "")) limit 10;
```
[*Disclaimer*](#disclaimer)

5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.

```
select message, sender, receiver, createdatetime from posts where sender = 1 and receiver = 51 and visibility = "TRUE" and message is not "" order by createdatetime desc limit 10;
```
[*Disclaimer*](#disclaimer)

6. Make all Stories that are more than 24 hours old invisible.

```
update posts set visibility = "FALSE" where ROUND((JULIANDAY('now') - JULIANDAY(posts.createdatetime)) * 24) > 24 and story is not "";
```
[*Disclaimer*](#disclaimer)

7. Show all invisible Messages and Stories, in order of recency.
```
select message, story, createdatetime from posts where visibility = "FALSE" and ((story is not "") or (message is not "")) order by createdatetime desc limit 20;
```
[*Disclaimer*](#disclaimer)

8. Show the number of posts by each User.

```
select handle, count(posts.id) as num_posts from users left join posts on users.id = posts.sender group by handle;
```

9. Show the post text and email address of all posts and the User who made them within the last 24 hours.

```
select message, story, users.email, users.handle from posts inner join users on posts.sender = users.id where ROUND((JULIANDAY('now') - JULIANDAY(posts.createdatetime)) * 24) < 24 and ((story is not "") or (message is not ""));
```
[*Disclaimer*](#disclaimer)

10. Show the email addresses of all Users who have not posted anything yet.

```
select email from users left join posts on posts.sender = users.id where posts.message is "" and posts.story is "";
```
[*Disclaimer*](#disclaimer)


### Disclaimer
I found out that the null values in my dummy data from Mockaroo were actually empty strings("") instead of null. I used "" for my particular dummy data, but in a proper dataset, the word NULL would be used in its place instead.