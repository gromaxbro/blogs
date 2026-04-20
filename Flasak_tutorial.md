# Python Flask Cheatsheet
a cheatsheet so you dont have to remember everything

## Routes

A route in Flask is a way of mapping a specific URL path to a Python function (known as a view function) that handles the web request and returns a response

**BASIC TEMPLATE**:
```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return `<h1>hello</h1>`

if __name__ == "__main__":
    app.run(debug=True)
```



### URL & ROUTES
root:
```
@app.route("/")
```

taking variable:
```
@app.route('/add/<number>')
def add_func(number):
```
taking variable more than one:
```
@app.route('/add/<number>/<number>')
def add_func(number1,number2):
```
taking variable specific type:
```
@app.route('/add/<int:number>')
def add_func(number):
```

talking args from the requests:
`/add?age=10`
```
def add_func(number):
	name = requests.args
	// return dict
	specific = requests.args.get("age")
```

### POST Requests

only accept post request
```
@app.route('/hello',methods=["POST"])
def function():
````
only post and get request
```
@app.route('/hello',methods=["POST","GET"])
def function():
	if request.method = "GET":
		return "you got get request"
	if request.method = "POST":
		return "you got post request"
````


### TEMPLATES
you have to import `Flask,render_template`
add in starting
```
app = Flask(__name__,template_folders='templates')
```

structure:
```
project/
│
├── app.py
├── templates/
│   └── index.html
```
return a html file:
```
@app.route("/")
def home():
    return render_template("index.html")

```

transfering variables in html file:
```
def home():
	data = "hello"
    return render_template("index.html",mydata=data)
```
inside the html:
use `{{ variable  }}` when dynamic content or variables is passed
```
<p>{{mmydata}}</p>
```

we use ``{% ... %}`` (curly braces with percent signs) for if statements and for loops
for example a list is passed in mydata
we can do
```
{% for item in mydata %}
	{{item}}
{% endfor %}
```

using if funciton
```
{% if mydata == "hello" %}
	{{item}}
{% endif %}
```

**template inheritance**

for example you dont wanna use the starting stuff of the index.html or and what to use same in all we can use block ``{% block name %} ... {% endblock %}``
**block** :=  Used in your "base" or parent file (often called base.html or layout.html), a block defines a section that child pages can fill with their own content 

`base.html`
```
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    <nav>Navigation Bar</nav>
    
    <div class="main-content">
        {% block content %}{% endblock %} <!-- Content from child pages goes here -->
    </div>

    <footer>Site Footer</footer>
</body>
</html>

```

index.html
```
{% extends "base.html" %}

{% block title %}Home Page{% endblock %}

{% block content %}
    <h1>Welcome to the Home Page!</h1>
    <p>This text is specific to the index page.</p>
{% endblock %}

```

**Why use url_for?**
Instead of hardcoding a link like` <a href="/login">`, you use` {{ url_for('login') }}`.

### images & files
strucutre for using images,and static file
```
├── static/            
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── script.js
│   └── img/
│       └── logo.png

```
use this for 
```
app = Flask(__name__,template_folders='templates',static_folder='static',static_url_path='/')
```
insde html you wanna use image
```
<img src="/img/logo.png">
```
