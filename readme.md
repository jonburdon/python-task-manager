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

* Use jinja formatting used before within UL tags.
* Wrap a list item within jinja tags in a For loop.
* Add task description jinja tag in collapsible part of accordion.
* copy and paste icon code for 'expand' google icon

### Add additional categories to database so these can be accessed with Flask

### Add UI for Add Task function

* Create addtask.html as child content of base.html
* Add route for add_task
* Define function for add task

###

* Copy form html from: https://materializecss.com/text-inputs.html
* Paste into the endblock in addtask.html

### Customising the form

Change col width to 12 from 2 x 6.

Note: Each element has an input field, a label and an image. The input-field class from Materialize provides the styling.

Alter input element's id, name and type. Match name to the associated field name in mongodb.

Change icons to reflect the information we expect.

Change text area input to html5 textarea element.

Put each input field in it's own row.

Add new row for checkbox input.

Paste code for checkbox from: https://materializecss.com/switches.html (Materialize calls them Switches)
Note: checkboxes only submit form data if they are checked.

Add id and name *is_urgent* to the checkbox

Copy and paste the Task Name row html. Reformat as Datepicker: Use class="datepicker" to add a date picker. Change icon.

Add Datepicker jquery to base.html

Update icon prefix in addtask.html to a poll.
Change label to task category

In app.py, find categories to render:

    categories=mongo.db.categories.find())

In addtask.html use for loop to create conditional rendering - each instance of a category in the collection will now be rendered. Set option value to category name to display categories. The option value can then be submitted to the form when the use selects it.

Button code for submit button taken from:
https://materializecss.com/buttons.html
Added to addtask.html at end of form, within new row.

Create insert_form function in app.py to post form data to mongodb.

Specify form action in addtask.html line 4

    <form action="{{ url_for('insert_task') }}" method="POST" class="col s12">



