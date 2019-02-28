# ORM Discussion Questions

Consider the following questions with the people at your table. Write down your answers in a notebook or on a whiteboard.

1. First, discuss with your table - what is an ORM? Why is an ORM useful?

-Object Relational Managament! Lets us not have to worry about the messiness of messing with a database, allows us to write everything we need to interface with the database, in our back-end code. Don't have to worry about losing our data now.

2. Pretend that you're building Twitter. Let's say that a tweet has a message and belongs to a User. A User has a username and has_many tweets. What columns would be on those two tables?

CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  username TEXT,
  location TEXT,
  profile_picture TEXT,
  )

CREATE TABLE tweets (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  message TEXT,
  post_time DATE,
  )

CREATE TABLE tweet_likes (
  id INTEGER PRIMARY KEY,
  user_id INTEGER,
  tweet_id INTEGER,
  )

3. Now that we have our tables, pretend that we are defining a method on our User class that returns all the tweets. What SQL fires when this method is called?

def find_all_tweets
  DB[:conn].execute("SELECT * FROM tweets
  JOIN users ON tweets.user_id = users.id
  WHERE tweets.user_id = #{self.id};")
end

4. Writing out all of these database interactions by hand can be messy. How would you create a method on the superclass to make sure the correct SQL fires for each class?

```
class LiveRecord

  def self.all
    sql = <<-SQL
      SELECT * FROM ?
    SQL
    DB[:conn].execute(sql, self.table_name)
  end
end

class Dog < LiveRecord
  @@all
  @@table_name = dogs

end

class Cat < LiveRecord
  @@table_name = cats

end

Dog.all #=> 'SELECT * FROM dogs'
Cat.all #=> 'SELECT * FROM cats'
```

<p class='util--hide'>View <a href='https://learn.co/lessons/orm-discussion-questions'>Orm Discussion Questions</a> on Learn.co and start learning to code for free.</p>
