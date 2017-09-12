1. mysql> SELECT first_name, last_name FROM student;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Eric       | Ephram    |
| Greg       | Gould     |
| Adam       | Ant       |
| Howard     | Hess      |
| Charles    | Caldwell  |
| James      | Joyce     |
| Doug       | Dumas     |
| Kevin      | Kraft     |
| Frank      | Fountain  |
| Brian      | Biggs     |
+------------+-----------+
10 rows in set (0.00 sec)



2. mysql> SELECT * FROM student WHERE years_of_experience < 8;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          1 | Eric       | Ephram    |                   2 | 2016-03-31 |
|          2 | Greg       | Gould     |                   6 | 2016-09-30 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
|          4 | Howard     | Hess      |                   1 | 2016-02-28 |
|          5 | Charles    | Caldwell  |                   7 | 2016-05-07 |
|          8 | Kevin      | Kraft     |                   3 | 2016-04-15 |
|         10 | Brian      | Biggs     |                   4 | 2015-12-25 |
+------------+------------+-----------+---------------------+------------+
7 rows in set (0.00 sec)



3. mysql> SELECT last_name, start_date, student_id FROM student ORDER BY last_name ASC;
+-----------+------------+------------+
| last_name | start_date | student_id |
+-----------+------------+------------+
| Ant       | 2016-06-02 |          3 |
| Biggs     | 2015-12-25 |         10 |
| Caldwell  | 2016-05-07 |          5 |
| Dumas     | 2016-07-04 |          7 |
| Ephram    | 2016-03-31 |          1 |
| Fountain  | 2016-01-31 |          9 |
| Gould     | 2016-09-30 |          2 |
| Hess      | 2016-02-28 |          4 |
| Joyce     | 2016-08-27 |          6 |
| Kraft     | 2016-04-15 |          8 |
+-----------+------------+------------+
10 rows in set (0.00 sec)



4. mysql> SELECT * FROM student WHERE first_name IN ('Adam', 'James', 'Frank') ORDER BY last_name DESC;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          6 | James      | Joyce     |                   9 | 2016-08-27 |
|          9 | Frank      | Fountain  |                   8 | 2016-01-31 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
+------------+------------+-----------+---------------------+------------+
3 rows in set (0.00 sec)



5. mysql> SELECT first_name, last_name, start_date FROM student WHERE start_date BETWEEN '2016-01-01' AND '2016-06-30' ORDER BY start_date DESC;
+------------+-----------+------------+
| first_name | last_name | start_date |
+------------+-----------+------------+
| Adam       | Ant       | 2016-06-02 |
| Charles    | Caldwell  | 2016-05-07 |
| Kevin      | Kraft     | 2016-04-15 |
| Eric       | Ephram    | 2016-03-31 |
| Howard     | Hess      | 2016-02-28 |
| Frank      | Fountain  | 2016-01-31 |
+------------+-----------+------------+
6 rows in set (0.00 sec)


Changing the GRADE field in Assignment

mysql> CREATE TABLE `assignment` (
    ->   `assignment_id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    ->   `student_id` int(11) NOT NULL,
    ->   `assignment_nbr` int(11) NOT NULL,
    ->   `grade` varchar(30) DEFAULT NULL,
    ->   `class_id` int(11) DEFAULT NULL,
    ->   PRIMARY KEY (`assignment_id`)
    -> ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.02 sec)

mysql> alter table assignment drop column grade;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table assignment add column grade_id int;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0



Add grade table:
mysql> CREATE TABLE `grade` (`grade_id` int(11) NOT NULL, `grade_values` varchar(30) NOT NULL, PRIMARY KEY (`grade_id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.01 sec)




Add foreign key to assignment table:
mysql> ALTER TABLE assignment ADD FOREIGN KEY (grade_id) REFERENCES `grade`(`grade_id`);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0




Add values in grade table:
mysql> insert into grade (grade_id, grade_values) values (1, 'Incomplete');
Query OK, 1 row affected (0.01 sec)

mysql> insert into grade (grade_id, grade_values) values (2, 'Complete and unsatisfactory');
Query OK, 1 row affected (0.00 sec)

mysql> insert into grade (grade_id, grade_values) values (3, 'Complete and satisfactory');
Query OK, 1 row affected (0.00 sec)

mysql> insert into grade (grade_id, grade_values) values (4, 'Exceeds expectations');
Query OK, 1 row affected (0.00 sec)

mysql> insert into grade (grade_id, grade_values) values (5, 'Not graded');
Query OK, 1 row affected (0.01 sec)




Add values in assignment table:
mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (2, 4, 6, 1);
Query OK, 1 row affected (0.01 sec)

mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (1, 3, 5, 2);
Query OK, 1 row affected (0.01 sec)

mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (6, 4, 2, 3);
Query OK, 1 row affected (0.01 sec)

mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (5, 3, 1, 4);
Query OK, 1 row affected (0.00 sec)

mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (1, 2, 3, 5);
Query OK, 1 row affected (0.01 sec)




Commands:
mysql> EXPLAIN assignment;
+----------------+------------------+------+-----+---------+----------------+
| Field          | Type             | Null | Key | Default | Extra          |
+----------------+------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11)          | NO   |     | NULL    |                |
| assignment_nbr | int(11)          | NO   |     | NULL    |                |
| class_id       | int(11)          | YES  |     | NULL    |                |
| grade_id       | int(11)          | YES  | MUL | NULL    |                |
+----------------+------------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)

mysql> SELECT * FROM assignment;
+---------------+------------+----------------+----------+----------+
| assignment_id | student_id | assignment_nbr | class_id | grade_id |
+---------------+------------+----------------+----------+----------+
|             1 |          2 |              4 |        6 |        1 |
|             2 |          1 |              3 |        5 |        2 |
|             3 |          6 |              4 |        2 |        3 |
|             4 |          5 |              3 |        1 |        4 |
|             5 |          1 |              2 |        3 |        5 |
+---------------+------------+----------------+----------+----------+
5 rows in set (0.00 sec)

mysql> EXPLAIN grade;
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| grade_id     | int(11)     | NO   | PRI | NULL    |       |
| grade_values | varchar(30) | NO   |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM grade;
+----------+-----------------------------+
| grade_id | grade_values                |
+----------+-----------------------------+
|        1 | Incomplete                  |
|        2 | Complete and unsatisfactory |
|        3 | Complete and satisfactory   |
|        4 | Exceeds expectations        |
|        5 | Not graded                  |
+----------+-----------------------------+
5 rows in set (0.00 sec)




Hard Mode:
Created Index:

mysql> CREATE INDEX idx_student_id ON assignment(student_id);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0



Made the student_id in assignment and student "the same":

mysql> ALTER TABLE assignment MODIFY student_id int(11) unsigned DEFAULT NULL;
Query OK, 5 rows affected (0.02 sec)
Records: 5  Duplicates: 0  Warnings: 0



Added constraint and foreign key:

mysql> ALTER TABLE assignment ADD CONSTRAINT fk_student_id FOREIGN KEY (student_id) REFERENCES student(student_id);
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0



Tested constraint by trying to add a student ID that doesn't exists to an assignment and got expected error (yay):

mysql> insert into assignment (student_id, assignment_nbr, class_id, grade_id) values (1001, 2, 3, 5);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`tiy`.`assignment`, CONSTRAINT `fk_student_id` FOREIGN KEY (`student_id`) REFERENCES `student` (`student_id`))
mysql>
