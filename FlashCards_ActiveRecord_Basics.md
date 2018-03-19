## ActiveRecord Basics

### What is ActiveRecord's naming convention?

Your model class name should be in `UpperCamelCase`, singular form (e.g. `SportsCar`).

Your table name should be in `lower_snake_case`, plural form (e.g. `sports_cars`).

ActiveRecord's magical 1:1 mapping between records of your DB and instances of your models relies entirely on this convention!

### What should prefix every migration filename?

A timestamp (format: YYYYMMDDHHMMSS)! For instance: `db/migrate/20140725164644_create_restaurants.rb`

This way, your files are properly ordered in your `migrate` folder.

### the following migration to create a `doctors` table with a `name` and a `specialty`

```ruby
class CreateDoctors < ActiveRecord::Migration[5.1]
  def change
    create_table :doctors do |t|
      t.string     :name
      t.string     :specialty
      t.timestamps # adds 2 columns, `created_at` and `updated_at`
    end
  end
end
```

### Fetch a doctor of a given specialty

```Ruby
surgeons = Doctor.where(specialty: "Surgeon")
# => returns a collection (~ array) of all surgeons.
You can also pass a string as an argument to the .where class method:
young_doctors = Doctor.where("age < 35")
# => returns a collection (~ array) of all doctors under 35 years old.
surgeons = Doctor.where("specialty LIKE %surgery%")
# => returns a collection (~ array) of all doctors with 'surgery' in their specialty.
dentists_and_surgeons = Doctor.where(specialty: ["Dentist", "Surgeon"])
```

### the following migration to add an `age` column to the `doctors` table

```Ruby
class AddAgeToDoctors < ActiveRecord::Migration[5.1]
  def change
    add_column :doctors, :age, :integer
  end
end
```

### the ActiveRecord query called based on the following SQL query read in your application logs:

```Ruby
SELECT * FROM restaurants WHERE rating >= 4 ORDER BY rating ASC LIMIT 5;
Restaurant.where("rating >= 4").order(rating: :asc).first(5)
```

### How do you retrieve all instances of a given ActiveRecord model?

```Ruby
By calling .all on your model!
doctors = Doctor.all
# => returns a collection (~ array) of all Doctor instances.
```

### How do you run pending migrations? By running the following in your terminal:

```Ruby
rake db:migrate
```

### what is a migration

You create a migration when you need to change your database's **schema** (add / remove table), (add / remove column), etc...

It's a change in the **structure** of the database, but it leaves the data unchanged.