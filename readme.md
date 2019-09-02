# Task Manager mini project

This project was built following a Code Institute tutorial.

The procedure was adapted to use VScode on a mac as the development environment, rather than Cloud9 as show in tutorial.

This is the developers own documentation for the project.

## Project Aims
1. Simple task manager app with web user interface for CRUD operations
2. Task database containing:
* Task Category
* Task Name
* Task Due Date
* Task description
* Task urgency (on or off)
3. Ability to edit, add and delete categories for tasks.
4. Project is responsive


## Developer Aims
1. Learn to use CRUD calls to mongodb using Python with Flask application
2. Build HTML based user interface to demonstrate CRUD calls in action
3. Style the above using the Materialize framework for improved UX
4. Document the project for future reference

## Technologies
* HTML
* Python
* Mongodb
* Flask
* Materialise


### Database Setup

Create database on mongodb cloud

Get data in place first, on atlas mongo website:
* database with two collection: categories and tasks
* Create a sample record in category collection called category_name: "Home"
* Create sample record in the task collection with category_name, task_name, task_description, is_urgent and due_date


### Setup using VS Code on a Mac
* **sudo pip3 install Flask** to install Flask
* **python3 -m venv env** to install virtual environment in that folder
* Open command palette, type **Python: Select Interpreter** and select the virtual environment in your project folder that starts with ./env or .\env
* In command pallette, run **Terminal: Create New Integrated Terminal**
* Install Flask in the virtual environment with **pip3 install Flask**
* **pip3 install flask_pymongo**
* Create app.py (In flask, the convention is to use run.py or app.py)
* **from flask import Flask** to import Flask in app.py Capital F indicates a class name.
* Create instance of flask within app.py with **app = Flask(name)**
* In Terminal **python3 -m flask run** to run the app and serve

### Deploying to Heroku
#### Four steps to deploying in Heroku
1. Create Heroku app
2. Link git repo
3. Create requirements.txt
4. Create Procfile


In terminal:

    heroku login

Use browser to log in

    heroku apps

In terminal

    git init
    git add .
    git commit -m 'Initial commit'
    heroku git:remote -a task-manager-learning-project
    git push heroku master (fails - needs requirements.txt)
    sudo pip3 freeze --local > requirement.txt
    git add .
    git commit -m 'Added requirements'
    git push heroku master (fails - needs procfile)
    echo web: python app.py > Procfile
    git push heroku master
    heroku ps:scale web=1

On Heroku web interface:
Specify IP 0.0.0.0 and Port 5000

### Connecting to MongoDB Atlas

    sudo pip3 install flask-pymongo
    sudo pip3 install dnspython

in app.py:

    from flask_pymongo import PyMongo
    from bson.objectid import ObjectId

    app.config["MONGO_DBNAME"] = 'task_manager'
    app.config[MONGO_URI] = '<<CONNECTION STRING WITH DB NAME AND PASSWORD'

On Mongo Website: Overview -> Connect -> Connect my app -> Choose Python3.6 or later

Copy connection string

Paste in connection string

**Ensure an environment variable for above is used when in production**
**Database mini project was set up using an environment variable**

Create an instance of pymongo, add app with constructor method.

    mongo = Pymongo(app)

Note: Routing is a string that when attached to  url will redirect to a particular flask function within an application

Add route for /get_tasks

    from flask import Flask, render_template, redirect, request, url_for

Rename hello functon to get_tasks and render template tasks.html

Note: When the application is run, the default function to be called will be get tasks because of the / decorator

Create template:
*Create templates directory and file tasks.html

Set body as:

    *{% for tasks in tasks %}
        {{tasks.task_name}}
        {{task.category_name}}
        {{tasks.task_description}}
        {{tasks.is_urgent}}
        {{task.due_date}}
    {% endfor %}*


Refactor so that tasks.html extends base.html template

### Adding Materialize to base.html
* Add Materialize css link in head
* Add jquery and javascript links in footer.
* Add Google icons link to header.

### Adding an accordian from Materialize

* Add 'Collapsible' component from Materialise website:
* https://materializecss.com/collapsible.html
* Copy and paste the sample html in to tasks.html
* paste jquery initialization code in the base.html template.

### Binding data from database to the Accordion elements.

Use jinja formatting used before within UL tags.
Wrap a list item within jinja tags in a For loop.
Add task description jinja tag in collapsible part of accordion.