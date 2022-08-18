10/08/2022

Todo App/Database File

1.	Installed SQLAlchemy to application, using ‘pip install sqlalchemy’.
2.	Created new folder for Todo application.
3.	Created database file for connecting to SQLite database, called todos.db (this file stores all the data for the application).
4.	Created an engine for SQLAlchemy.
•	To be able to first create the engine, create_engine had to be imported from sqlalchemy
•	To be able to create a new database session, use of the sessionmaker function is required, which meant sessionmaker had to be imported from sqlalchemy.
5.	Created session maker.
6.	Created base which allows creation of each database model, as a base class can be inherited in future files.
•	To be able to create a base, declarative_base needed to be imported from sqlalchemy.ext.declarative
7.	Initiated git repo inside FastAPI folder.

Models File

1.	Creates the table and column structure of what the database is going to look like.
2.	A table called 'todos' was created within the file.
3.	A table can be created within a SQLite DB by just defining a class of todos, which extends Base.

Main File

1.	Main file for Todo app to create all the APIs.
2.	After creation of main file and when starting uvicorn server, the todos.db file was automatically created within the Todo App Folder.

Installation of SQLite3

1.	Installed, added to path in System Environment Variables, then checked to see if sqlite3 worked from the command prompt.

Manipulated SQLite3 DB Using Terminal

1.	Used ‘sqlite3 todos.db’ command to start database for data entering and manipulation.
2.	Inserted 3 todos into table.

API Request Methods

1.	Set a function in main.py which allows all todos to be retrieved from the database – GET/Read All API.
2.	Created API for retrieving/getting todos by ID (Primary Key).
3.	Created POST request method for todos – allows creation of new todos from an API, and then we can save and retrieve/fetch those todos from the database.
4.	Created PUT request method for Todo App – allows the user to identify todo by the ID and enhance it by a todo request body.
5.	Created DELETE Request Method – deletes the todo record where the ID record matches the Todo ID

11/08/2022

Authentication and Authorization

1.	Enhanced Todos table and created database table for Users in models.py file.
2.	Created a relationship between the two tables using a PK and FK
3.	Viewed the tables in sqlite3 todos.db 
4.	Created new file called auth.py, then created a POST request method in auth.py, which creates a new user.
5.	Hashed passwords by using a password encryption of Bcrypt.
•	Downloaded the popular password hashing framework passlib, using ‘pip install “passlib[bcrypt]”’
•	imported passlib.context from CryptContext
•	Tested whether the passwords were hashed in the POST API, and the bcrypt encryption method successfully returned a hashed password.
6.	Saved a user to the database using the POST API, then viewed the data (which also contained the hashed password) in sqlite3 todos.db.
7.	Authenticated users who sign in. This was done by verifying the new password that they typed in and log in with, with the new hashed password.
•	Returned whether the authentication process worked or not.
•	Imported OAuth2PasswordRequestForm from fastapi.security, which is what users use to sign in with.
8.	Created a JWT – JSON Web Token
•	If a user signs in successfully, a JWT will be returned to them that they can then attach to future APIs for authorization – this is the most modern way of dealing with authentication and authorization
•	First, had to install the python library “python-jose[cryptography”] with pip
•	After the successful installation, OAuth2PasswordBearer was imported from fastapi.security because JWT is a bearer token – bearer is a type of token and authorization platform
•	Datetime and timedelta were imported from datetime so that expiration dates could be added to the token itself
•	Jwt was imported from jose, which is what had just been downloaded prior to doing imports
•	Added a secret key in auth.py, which is the final level of security on a JWT and is really the signature that goes with the JWT token
•	Tested the token in the POST token API
9.	Created a new function called ‘get_current_user’ which is going to be able to decode a JWT token – this allows us to validate what user and id is inside of the JWT. This is how you authorize when sending you’re tokens in without the APIs.
10.	Replaced all HTTPExceptions in auth.py with functions that return their own types of exceptions to make the code significantly cleaner.

Authenticate Requests

11.	Downloaded Postman which is an API software and platform for using APIs - it is the industry standard for testing APIs.
•	Postman comes with many tools for design and testing of APIs
12.	Set up database with user and todo relationships.
•	Created a new user with the POST create_user API
•	Used database to create 3 new todos and linked the todos to both users in the database (from users table)
13.	Get todos by user id:
•	Ran the auth app with uvicorn on port 9000 so that the main app could be run on the normal port 8000
•	Signed into the POST Login API in the auth app then got (copied) a JWT
•	Used the GET Request Method in Postman with the main app url (port 8000) 
•	Passed the JWT as the authorization bearer token
•	Validated the token
•	Then returned the todos that belonged to the user
14.	Enhanced the read_todo function in main.py to now take in a user id or a user JWT to validate a user before returning a todo id.
15.	Enhanced create_todo function to now take in a user’s JWT, authenticate the JWT, return a get_user_exception if the user is None, then enhance the todo_model to now include a new id.
16.	Enhanced update_todo function in main.py to now take in a user JWT, validate the user, enhance the todo_model to find the id where a user id also matches the owner id, then submit the PUT Request.
17.	Enhanced the delete_todo function in main.py to now take in a user’s JWT, authenticate the user, and to delete a todo based on the ID and the user id.

Production Database Setup

1.	Downloaded PostgreSQL and its accompanying GUI, PGAdmin4.
•	Created server in Postgres called ‘TodoApplicationServer’
•	Created database in TodoApplicationServer called ‘TodoApplicationDatabase
•	Created two tables within the TodoApplicationDatabase called ‘users’ and ‘todos’.
2.	Connected FastAPI Application to PostgreSQL database (all code modified/added was done in database.py)
•	First, installed psycopg2-binary with pip – this is one of the main ways that the application will be able to connect to the PostgreSQL database
•	Changed the SQLALCHEMY_DATABASE_URL from “sqlite:///./todos.db” (in the database.py file) to “postgresql://postgres:********@localhost/TodoApplicationDatabase”, to point the FastAPI application towards the PostgreSQL database
•	Deleted/removed the argument “connect_args={"check_same_thread": False}” from the database engine because it is a specific argument to Sqlite 
•	Created 2 new users in the Create_New_User API, then viewed the new users in the users table of of the TodoApplicationDatabase in Postgres – this showed that connection of the database to FastAPI was successful.
3.	Created todos for the FastAPI application, saved the todos to PostgreSQL database and matched foreign keys of todos to the 2 users which had been created earlier. Creation and insertion of data was successful.

12/08/2022

MySQL DBMS

1.	Downloaded MySQL
2.	Created a todoapp database 
•	Created 2 new tables inside the database, called ‘users’ and ‘todos’
3.	Connected Todo App to todo database using MySQL and FastAPI
•	First, installed a new dependency called ‘pymysql’ within the FastAPI environment, using pip
•	[Assume that this changes from sqlite to mysql, rather than from postgresql to mysql] Changed the SQLALCHEMY_DATABASE_URL from “sqlite:///./todos.db” (in the database.py file) to “mysql+pymysql://root:Test1234@127.0.0.1:3306/todoapp”
4.	Created two new users in the auth app
•	Selected users table in MySQL to view the new insertions in users table
5.	Created todos to go into mysql database 
•	Retrieved tokens from both users in the auth APIs
•	Created todos for each user in Postman
•	Viewed the new insertions in mysql to verify successful POST Requests
•	Used GET Requests also

Routing

1.	Created and switched to a new git branch from the master/main branch called ‘routing’.
2.	Created a directory within the TodoApp called routers, then put the auth.py file in routers.
•	Removed FastAPI dependency and imported APIRouter from fastapi (within auth.py)
•	Changed @app decorators to @router
•	Created a todos.py file within the routers directory to hold all of the todos information, transferred most of the code over from main.py. The todos.py file was added as another router within the main.py file, which is the application that’s getting deployed on the uvicorn
•	Deleted most of the code from main.py
•	All @app decorators were changed to @router for scalability of the application.
3.	Added tags and prefixes as responses to the routers.

Organized APIs with prefixes and tags:

4.	I was not able to start up the uvicorn server from the time I had started the routing process until prefix="/auth", tags=["auth"], responses={401: {"user": "Not authorized"}} were passed in as parameters to router = APIRouter() within the auth.py file.
5.	Also passed in prefix="/todos", tags=["todos"], responses={404: {"description": "Not found"}} as parameters to router = APIRouter() within the todos.py file.
6.	External Routing: Created a company directory and within the directory, created a companyapis.py file. 
•	Rather than passing in tags, prefixes and responses to the router within the companyapis.py file, this was instead done in the main.py file.
7.	Created a new users.py file within the routers directory.
•	Created 3 GET APIs
•	Created 1 PUT APIs
•	Created 1 DELETE APIs

Code Clean Up
1.	Deleted company directory
2.	Deleted users.py

Full Stack

1.	Merged the routing branch into master
2.	Created a new branch called full_stack from the HEAD of Master
3.	Deleted the routing branch
4.	All work for full stack is to be done in the full_stack branch until completed
5.	Created a new directory called templates
6.	Within templates, created an HTML file called home
7.	Installed aiofiles and jinja2 with pip
8.	Added a new GET router API in todos.py
9.	Returned a successful HTML front end page
10.	Created a new directory called static, then a directory inside of that called todo
11.	Created 2 directories called css and js inside of the todo directory
12.	Created a file called base.css inside of css directory
13.	Added base and bootstrap files to the css directory
14.	Added bootstrap, jquery-slim and popper files to the js directory – all required files to get the bootstrap javascript rendering properly. Bootstrap is a framework that allows us to deal with css and js extremely easy as there are a whole bunch of pre-built components.
15.	Used Jinja, which is a templating language that is able to write code similar to Python in the DOM. Jinja templating tags and scripts allows developers to be confident working with backend data, using tags that are similar to HTML.
16.	Created 5 html files within the templates directory – add-todo, edit-todo, home, login and register.
17.	Deleted much of what was in todos.py
18.	Created API requests and Router requests to redirect response to html files

14/08/2022

1.	Created a new file within the templates directory called layout to be able to create and export html code into more manageable pieces of code.
•	At the top of each html file (except layout.html), {%include 'layout.html' %} was added at the top of each file.
•	Much of the code from login.html was cut and pasted into layout.html
•	Much of the code was deleted from home.html
•	The same code was deleted from add-todo.html, edit-todo.html and register.html.
2.	Created a new navbar.html file, which contains the navbar component (navbar code was cut and pasted from layout.html). Layout.html file is going to have all of the predefined code.
3.	Modified todos.py and home.html to be able to update a user using a User

Web pages now have functionality, redirecting the user to different pages of the application when clicking on buttons

4.	Modified todos.py and add-todo.html to be able to add a new todo (with a title, description and priority) and then redirect the user back to the home todos page (which shows all of the user’s todos).
5.	The home.html file was then modified to allow the user to be able to click on the ‘Add a new Todo’ button from the home page, in order to redirect back to the add-todo page.
6.	Modified todos.py and edit-todo.html to be able to edit each individual todo.
7.	The POST Request method was added for edit-todo functionality.
8.	The delete functionality was added to the application.
9.	The complete functionality was added to the application, where the user can now click on the complete/undo button for each todo. If complete button is clicked, a strike-through appears through the todo and the background of todo turns green.
10.	Modified auth.py  and login.html file:
•	Created 2 functions within a class called ‘loginform’
•	Created a new POST API with a function called ‘login’
•	Changed expiry time for token from 20 minutes to 60 minutes
•	Set a cookie within the POST token API
11.	Added logout functionality
12.	Added register functionality
13.	Cleaned code by deleting code in auth.py
14.	Cleaned more code:
•	Changed auth.py file to return HTTP Exception
•	Changed navbar to return back to slash (/)
•	Added main.py which is going to be a slash to redirect to todos
•	Removed the todoapp folder and put everything in the parent directory of FastAPI

16/08/2022

15.	Enhanced application so a user can change their password.
•	Created new template for change password called ‘edit-user-password.html’
•	Created a new users.py file within the routers directory
•	Created new route for password change called users.py
•	Added ability to verify password and then change the password

18/08/2022

1.	Fixed small errors in auth.py and todos.py which were preventing the login and edit functionalities from working.
2.	Updated the delete functionality code in edit-todo.html and was able to delete todos successfully.
3.	Merged the full_stack branch back into the main branch.
4.	Added (missing) code to edit-user-password.html, in order to complete the functionality where a user is able to change their password and create a new password.
5.	Pushed the application to GitHub.
6.	Deleted the full_stack branch.

Deployment

1.	Created Heroku account and connected GitHub account to Heroku.
•	Connected Heroku to the specific repository
2.	Created the files that Heroku is dependent on to deploy on to a dyno (virtual machine), in order for the application to go live.
•	Created a file called runtime.txt in the parent FastAPI directory. This file simply contains the version of Python that I wanted Heroku to install on its dyno for the application to run properly
•	Created a file called requirements.txt, which holds all of the dependencies for the application. I used a quick trick where I typed ‘pip freeze’ into the terminal, which showed all the dependencies for the entire application. I copied the dependencies and pasted them into requirements.txt
•	The last file created was a Procfile, which is very specific to Heroku. This file specifies certain things for Heroku to know what the application is and exactly how it is to be deployed.
•	Added these 3 files (plus this README.md file) to the main branch and then pushed the main branch to the git repo.

