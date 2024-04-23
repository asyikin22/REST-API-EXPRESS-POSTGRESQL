# Install PostgreSQL 

**How to Install**:
* https://www.postgresql.org/download/windows/
* Password: XXXXXX
* Port: 5432
* PgAdmin
* Host: LocalHost (127.0.0.1)
* User: postgres

**Setting up PostgreSQL**
* Open SQL Shell (psql)
* My password authentication failed, I disable the password using this guide from stack overflow
* Link: https://stackoverflow.com/questions/55038942/fatal-password-authentication-failed-for-user-postgres-postgresql-11-with-pg

# SETTING UP DATABASE

**Confirm connection to PostgreSQL:** 
* Type "\conninfo"

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/d9da99c8-5e94-4e09-9ae1-82e00a8c25e1)

**To see existing database and create new one:**
* Type "\l"
* Create a new database: " CREATE DATABASE students"
* 'Student' database created 2nd from bottom of the list

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/6014a93c-0f66-4167-90dc-f64159dbfc9c)

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/7777ed65-afdc-476c-8ed0-d0ffc93c7a41)

**Connect to database to create a table: "\c students"**

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/2724d7f9-a543-4d84-9c4d-5818a69a09c1)

**CREATE TABLE students**
* ID - SERIAL as it will auto-increment every time we create students starting from 1.
* Name
* Email
* Age
* Date of birth
* \dt = display table

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/3103d0fa-6a9c-429a-abb7-ad39b86c92cc)

**CREATE NEW STUDENTS**
* INSERT INTO students (name, email, age, dob)
* VALUES (####)
* Display table: # SELECT * FROM students

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/f8eb1a23-fdc4-4ff0-bec1-48a1ec5b6b2d)

# pg-pool package

**WHAT IS IT?**:
* It's a connection pool for the pg package, specifically designed to manage and reuse PostgreSQL client connections efficiently.
* It is a popular Node.js library for interacting with PostgreSQL databases.
* It provides a simple and efficient way to execute SQL queries, handle transactions, and manage connections to a PostgreSQL database.

**PASSWORD ISSUES**:
* I have changed the authentication method for PostgreSQL to 'trust', it means that the database server trusts me, effectively removing the password requirement for local connections. 

**Creating a "database connection pool" in Node.js**

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/51f6ecc2-bdcf-4378-9ed6-58fd2ca826ee)


# VS CODE SET UP

**Create file structure within 'student' folder**
* Queries.js - this where we store all our SQL queries we're going to use against our database
* Routes.js - this is where we store our students routes
* Controller.js - this is where we store our business logic that’s related to each route

**Create a student route in routes.js**
* Define an Express.js router for handling API routes in routes.js file
* Import module inside main file and mount the student router at the '/api' path
* Test in Postman if it route is succesfully created.

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/2c045d5b-6f10-482d-bf1b-34ed4a717605)

**Get the students from the database - USING CONTROLLER**
* Instead of sending strings using API routes, we will now send students from DB back to the user
* When user ping this route '/', we want to query our DB, get the JSON response with our students and send it back to user
* Create a 'getStudents' function in controller file which we will use in over in our route file

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/8da1a07a-ccd5-429a-8b19-216b008ec04e)

# RETRIEVE DATA FROM POSTGRESQL INTO VS CODE

**Get the students from the database - IMPORT DB FROM DB.JS FILE**
* Using pool to query our DB
* Query method has 2 parameters
  1) SQL statement - statement used to query the DB
  2) Callbacks (error, results)
* Add MW in the main file to post and get JSON from our endpoints
* It's used to handle JSON data sent by clients in POST requests and makes the parsed JSON data available in req.body for further processing within route handlers. 

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/ef994cff-558a-463b-9a3f-4a63a4143220)

**Now we test it in Postman, we will get the two student data we created in PostgreSQL earlier**

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/f5a8c37f-d020-4a4f-96b4-2f824de19733)

**Separate out queries from the business logic inside controller file**
* As the data increases in size, we need to separate it from the controller file to organize the codes better and not get things mixed up
* We will shift the queries 'get Students into queries.js file and import it back to the controller file 

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/add467b3-60e8-4209-b1d3-2521cb3ff1eb)

# GET A SPECIFIC STUDENT BY ID 

**GET A SPECIFIC STUDENT BY ID - INSIDE PSQL**
* Type SELECT * FROM students WHERE id = 1
* It will return student with id number 1.

**GET A SPECIFIC STUDENT BY ID - INSERT THE LOGIC INTO QUERY FILE**
* Add new queries inside query file - getStudentsById
* Inside router file, create a new route
* Inside controller file, create that route

**ISSUES**
* Error
* On controller file: router.get(':/id', controller.getStudentById);
* It should be '/:id'. I am a freaking idiot lol

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/4d1a3bec-6312-46b3-b973-bc66d388dd84)

# ADD STUDENT
* Create a route to add new student in router file
* Create request body and use JavaScript destructuring to get new details out of the body - inside the controller file
* We have to check if we already have the student in the database and if it's valid.
* Write query in query files

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/1b7cfe42-7b4f-4c7d-8fbe-8f10752bc901)

**ADD STUDENT - AFTER WE HAVE VERIFIED ITS EXISTENCE**

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/9b003b7a-b419-420c-ab0b-ca333cccfe0f)

**To confirm that we have added the new student (Anna), we can check:**
1) Postman: get request for all students
2) PSQL: SELECT * FROM students;

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/ed49a0b9-d9f7-4ee7-a00b-b0205efd5be0)


# DELETE STUDENT
* Create delete route in route file
* Create remove student function inside controller file - Check if the student exists in DB
* Add remove student function inside query file

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/8553ed90-ac4c-4d09-9f63-0444b189d4da)

# UPDATE STUDENT
*	Create update router in the route file
*	Create update student function in the controller file
*	Add update student function inside query file
*	NOTE: It should be student with ID=3, not ID=2. my bad.

![image](https://github.com/asyikin22/REST-API-EXPRESS-POSTGRESQL/assets/148519441/d29e5a20-9d14-4a1b-ba1d-75f6ac0b7af4)
















