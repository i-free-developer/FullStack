## Schema Design & SQL

1. A database schema contains tables corresponding to the models.Each table has a set of columns (corresponding to the model's instance variables).A new record saved will generate a new row in its table.

2. In a `doctors` table with a `specialty` column, write an SQL query to count doctors by specialty, in a descending order?

3. SELECT COUNT(*) as c, specialty FROM doctors GROUP BY specialty ORDER BY c DESC;

4. In a DB with a `students` table, write an SQL query to count all students by `age`? 

5. SELECT COUNT(*), age FROM students GROUP BY age;

6. join table : To modelize a **N:N** relation, you need to insert a join table between the 2 tables and bring it back to two **1:N** relations between each table and the join table. The join table will store in its columns the two foreign keys.

7. Sqlite is a simple DB format that fits the whole DB in one file. Great to quickly test, but not suited for production.

8. In a 3-table (doctors, patients, consultations) schema, write an SQL query to retrieve for each consultation:

   - consultation's date,
   - the patient's first name and last name,
   - the doctor's first name and last name?

   ```Ruby
   SELECT c.starts_at, p.first_name, p.last_name, d.first_name, d.last_name
   FROM consultations c
   JOIN patients p ON c.patient_id = p.id
   JOIN doctors d ON c.doctor_id = d.id;
   ```

9. a relational database: It's a set of **tables** (made of columns and rows) linked together by a system of **primary keys** / **foreign keys**

10. Join two tables in a query : SELECT * FROM first_tableJOIN second_table ON second_table.id = first_table.second_table_id;

11. returns all `doctors`having a specialty ending with "surgery" (note the `%`): SELECT * FROM doctors WHERE specialty LIKE "%surgery";

12. Sqlite3 is a console where you can type SQL queries on a sqlite DB.To create a `db.sqlite` DB you can type: sqlite3 db.sqlite . Then you can start typing SQL queries in the console.

13. ​