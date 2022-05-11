# Task - Safari Park

We've now seen how to set up and query a relational database, including how to set up links between our tables and query many tables at once. Now we'll put it into practice by building a database to store data in a specific format.

Usually we'll have a back-end application handling user input and communicating with the database. It will receive that input in a format known as **JSON** (**J**ava**S**cript **O**bject **N**otation) and determine how to structure a query based on that. In this task we will provide you with sample data which you should use as a guide to set up the tables. 

## The Data

Our users will be able to enter details for the animals in the park, their enclosures and the staff working there. Each member of staff will be assinged to a different enclosure on a different day; each enclosure will have more than one person looking after it. The data entered by the user will look like this:

```json
// animal

{
	"id": 1,
	"name": "Tony",
	"type": "Tiger",
	"age": 59,
	"enclosure_id": 1
}

// enclosure

{
	"id": 1,
	"name": "big cat field",
	"capacity": 20,
	"closedForMaintenance": false
}

// staff

{
	"id": 1,
	"name": "Captain Rik",
	"employeeNumber": 12345,
}

// assignment

{
	"id": 1,
	"employeeId": 1,
	"enclosureId": 1,
	"day": "Tuesday"
}
```

## MVP

- Draw an entity relationship diagram to show the structure of the tables and the relationships between them. Each table should have enough columns to capture all the data shown in the JSON above.
- Set up the tables in a postgres database. You can set them up using the `psql` REPL, a GUI like Postico or PGAdmin or by writing an SQL file like the one in the previous task.
- Populate the tables with some of your own data (you don't need to use more cereal mascots, unless you want to). Don't worry about the capacity restriction on enclosures for now, checking the would be handled by the back-end before the data gets sent to the database.

#CREATE TABLES
CREATE TABLE staff(

id SERIAL PRIMARY KEY, name VARCHAR(255), employeeNumber INT);

CREATE TABLE animal(
id SERIAL PRIMARY KEY, animalName VARCHAR(255), species VARCHAR(255), age INT, enclosure_id INT REFERENCES enclosure(id);

CREATE TABLE enclosure(
id SERIAL PRIMARY KEY, title VARCHAR(255), capacity INT, closedForMaintenance INT);

CREATE TABLE assignment(
id SERIAL PRIMARY KEY, employee_id INT REFERENCES staff(id), enclosure_id INT REFERENCES enclosure(id), weekday VARCHAR(255));

#INSERT
INSERT INTO enclosure(title,capacity,closedformaintenance) VALUES ('mammals',5,1); 

INSERT INTO animal(animalName, species, age, enclosure_id) VALUES ('dog','mammals',20,1); 
INSERT INTO animal(animalName, species, age, enclosure_id) VALUES ('cat','mammals',10,1); 
INSERT INTO animal(animalName, species, age, enclosure_id) VALUES ('monkey','mammals',25,1); 
INSERT INTO animal(animalName, species, age, enclosure_id) VALUES ('rat','mammals',15,1); 
INSERT INTO animal(animalName, species, age, enclosure_id) VALUES ('deer','mammals',23,1); 

INSERT INTO staff(staffName, employeenumber) VALUES ('Mohamed', 12345); 
INSERT INTO staff(staffName, employeenumber) VALUES ('Luffy', 56565); 
INSERT INTO staff(staffName, employeenumber) VALUES ('Michael Jackson', 35768); 

INSERT INTO assignment (employee_id, enclosure_id, weekday) VALUES ( 1, 1, 'Monday'); 
INSERT INTO assignment (employee_id, enclosure_id, wwekday) VALUES ( 2, 1, 'Monday'); 
INSERT INTO assignment (employee_id, enclosure_id, weekday) VALUES ( 3, 1, 'Monday'); 

INSERT INTO assignment(employee_id, enclosure_ id, weekday) VALUES (1,1,'Tuesday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (2,1,'Tuesday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (3,1,'Tuesday');

INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (1, 1,'Wednesday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (2, 1, 'Wednesday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (3, 1, 'Wednesday'); 

INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (1, 1, 'Thursday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (2, 1, 'Thursday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (3, 1, 'Thursday'); 

INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (1, 1, 'Friday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (2, 1, 'Friday'); 
INSERT INTO assignment(employee_id, enclosure_id, weekday) VALUES (3, 1, 'Friday');

- Write queries to find:
	- The names of the animals in a given enclosure
	SELECT animal.name 
	AS animal_name,enclosure.name 
	AS enclosure_Name,animal.enclosure_id 
	FROM animal 
	INNER JOIN enclosure 
	ON animal.enclosure_id = enclosure.id 
	WHERE enclosure_id = 1 
	- The names of the staff working in a given enclosure
	SELECT staff.name 
	AS staff_name , enclosure.name 
	AS enclosure_name ,assignment.enclosureid 
	FROM staff 
	INNER JOIN assignment 
	ON assignment.employeeid = staff.id 
	INNER JOIN enclosure 
	ON assignment.enclosureid=enclosure.id 
	WHERE assignment.enclosureid=1;

 

	
## Extensions

Write queries to find:

- The names of staff working in enclosures which are closed for maintenance
- The name of the enclosure where the oldest animal lives. If there are two animals who are the same age choose the first one alphabetically.
- The number of different animal types a given keeper has been assigned to work with.
- The number of different keepers who have been assigned to work in a given enclosure
- The names of the other animals sharing an enclosure with a given animal (eg. find the names of all the animals sharing the big cat field with Tony)

## Hints

- Don't be tempted to perform too many joins, think about the data stored in each table.
- Remember that the result of a join is just another table. It can be filtered, ordered, summarised and joined again as much as you need.