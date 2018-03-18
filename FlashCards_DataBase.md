Flash Cards on Database

### Draw the equivalence between models in ruby and tables in a DB?

```ruby
Ruby                  (ex)       DB                    (ex)
--------------------------  <=>  --------------------------
Model             (Doctor)       Table            (doctors)
Instance      (Doctor.new)       Row
Instance variable  (@name)       Column              (name)
```

### How do we call the structure of a relational database?

It's the schema. In sqlite3, you can run the `.schema` command to display it:

```Ruby
sqlite> .schema
CREATE TABLE `doctors` (
  `id`  INTEGER PRIMARY KEY AUTOINCREMENT,
  `name` TEXT,
  `age` INTEGER,
  `specialty` TEXT
);
```

**CRUD** stands for **C**reate, **R**ead, **U**pdate, **D**elete

### How do you **D**elete (CRU**D**) in SQL? Using the `DELETE` keyword:

```Ruby
DELETE FROM doctors WHERE id = 32;
Warning, if you ommit the WHERE statement, you will delete every record in your table!
```

### How do you **C**reate (**C**RUD) in SQL?

Using the `INSERT INTO` keyword: [Read more about INSERT](http://www.techonthenet.com/sql/insert.php)

```Ruby
INSERT INTO doctors (name, age, specialty)
VALUES ('Dr House', 42, 'Diagnostic Medicine');
We don't insert the id, because the database manages it automatically for us (autoincrement).
```

### How would you establish a connection with a DB using ruby and sqlite?

```Ruby
With the #execute method called on a SQLite3::Databaseinstance:
require 'sqlite3'
DB = SQLite3::Database.new("db/doctors.db")
rows = DB.execute('SELECT * FROM doctors')
```

### It's Dr House's birthday. What SQL query would you execute to update his age to its new value 42 knowing his id is 3?

```ruby
UPDATE doctors SET age = 42 WHERE id = 3;
```

### Describe the structure of a database in your own words?

- A schema is composed of **tables**.
- Each table has a set of **columns**.
- When inserting data in a table, you create a **record** in a new **row**.

### How do you **U**pdate (CR**U**D) in SQL? Using the `UPDATE` keyword:  [Read more about UPDATE](http://www.techonthenet.com/sql/update.php)

```ruby
UPDATE doctors SET age = 40, name = 'John Smith' WHERE id = 3;
Will update the name and age of the doctor with the id 3.
Be careful, if you omit the WHERE statement, you will update every record of table!
```

### How do you **R**ead (C**R**UD) in SQL? using the `SELECT` SQL keyword: [Read more about SELECT](http://www.techonthenet.com/sql/select.php)

```Ruby
SELECT * FROM doctors; #Fetch all doctors
SELECT * FROM doctors WHERE id = 3; Fetch the doctor with the id 3
```

### What `SQLite3::Database` instance method is useful to fetch last assigned id?

`#last_insert_row_id` returns the last id (automatically) assigned by the DB.

### Whose responsibility is it to set the id of a new model instance?

It's the DB's responsibility. Every DB manages the id's autoincrement.

In ruby, our job is to retrieve it from the DB and update our instance:

```Ruby
# instantiate doctor in ruby
john = Doctor.new(name: "John", age: 42)
# persist data in DB
DB.execute("INSERT INTO doctors (name, age) VALUES ('John', 42)")
# retrieve id from DB
id = DB.last_insert_row_id
# update ruby instance
john.id = id
```

### How would you make a `DB.execute(...)` call return results as hashes (instead of arrays)?

By setting its `results_as_hash` to `true`:

```Ruby
DB.results_as_hash = true
doctors = DB.execute("SELECT name, age FROM doctors")
# => [
#      { "name" => "John Smith", "age" => 39 , 0 => "John Smith", 1 => 39 },
#      { "name" => "Emma Reale", "age" => 31 , 0 => "Emma Reale", 1 => 31 }
#    ]
```

### Write a #save instance method to create or update a given post in the DB:

```Ruby
class Post
  def initialize(attributes = {})
    @id = attributes[:id] || nil
    @url = attributes[:url]
    @votes = attributes[:votes] || 0
    @title = attributes[:title]
  end

  def save
    # TODO: write this method using SQLite3::Database instance DB
  	end
  end
def save
  if @id.nil?
    DB.execute("INSERT INTO posts (url, votes, title) VALUES (?, ?, ?)", @url, @votes, @title)
    @id = DB.last_insert_row_id
  else
    DB.execute("UPDATE posts SET url = ?, votes = ?, title = ? WHERE id = ?", @url, @votes, @title, @id)
  end
end
```