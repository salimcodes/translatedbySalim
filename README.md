# Step-by-Step Hands-On Process for replicating the AI-translator created by Salim Oyinlola

What will you learn: How to create a Flask application, create a Translator service on Azure, and use requests to call the service.

What you'll need: Python and Visual Studio Code

## Setup your environment
We will start by configuring the environments. We will install the necessary tooling, create the folder for their project, and setup the necessary Python libraries.

To get started writing our Flask application with Python, we need to set up our development environment, which will require a couple of items to be installed. Fortunately, the tools we'll use are relatively common, so they'll serve you well even beyond this module. You might even have them installed! We'll use these tools to develop and test your application locally.

At a high level, we'll perform the following steps:

1. Install Visual Studio Code (if not already installed)
2. Install Python (if not already installed)
3. Create a directory for your code
4. Create a virtual environment
5. Install Flask and other libraries

### Install Visual Studio Code (if not already installed)

Visual Studio Code is an open-source code editor that allows you to create almost any type of application you might like. It's backed by a robust extension marketplace where you can find add-ons to help make your life as a developer easier.

### Install Python (if not already installed)

To complete this unit, you must have Python 3.6 or later installed on your computer. There's a chance you might already have Python installed, especially if you've already used it. You can confirm whether it's installed by executing one of the following commands:

```
python --version
```

### Create a directory for your project/code

Create a directory in the location of your choice. This directory will be your project directory, and will contain all of the code we'll create. You can create a directory from a command or terminal window with one of the following commands:

```
# Windows
md contoso
cd contoso

## macOS or Linux
mkdir contoso
cd contoso
```

### Create a virtual environment

A Python virtual environment isn't necessarily as complex as it sounds. Rather than creating a virtual machine or container, a virtual environment is a folder that contains all of the libraries we need to run our application, including the Python runtime itself. By using a virtual environment, we make our applications modular, allowing us to keep them separate from one another and avoid versioning issues. As a best practice you should always use virtual environments when working with Python.

To use a virtual environment, we'll create and activate it. We create it by using the venv module, which you installed as part of your Python installation instructions earlier. When we activate it, we tell our system to use the folder we created for all of its Python needs.

```
# Windows
# Create the environment
python -m venv venv
# Activate the environment
.\venv\scripts\activate

# macOS or Linux
# Create the environment
python -m venv venv
# Activate the environment
source ./venv/bin/activate
```

### Install Flask and other libraries

With our virtual environment created and activated, we can now install Flask, the library we need for our website. We'll install Flask by following a common convention, which is to create a `requirements.txt` file.

The `requirements.txt` file isn't special in and of itself; it's a text file where we list the libraries required for our application. But it's the convention typically used by developers, and makes it easier to manage applications where numerous libraries are dependencies.


During later exercises, we'll use a couple of other libraries, including `requests` (to call Translator service) and `python-dotenv` (to manage our keys). While we don't need them yet, we're going to make our lives a little easier by installing them now.

- Open the folder in Visual Studio Code.

- Create a new file.

- Name the file `requirements.txt`, and add the following text:

```
flask
python-dotenv
requests
```

- Save the file by clicking Ctrl-S, or Cmd-S on a Mac

- Return to the command or terminal window and perform the installation by using pip to run the following command:

```
pip install -r requirements.txt
```

Hurray!!! The command downloads the necessary libraries and their dependencies.


## Create the app
After setting up the environment, we will create the project. We will create the template for the landing page and test that the application is running correctly.

Typically, the entry point for Flask applications is a file named `app.py`. We're going to follow this convention and create the core of our application. We'll perform the following steps:

1. Create our core application
2. Add the route for our application
3. Create the HTML template for our site
4. Test the application

### Create our core application

Returning to the instance of Visual Studio Code we were using previously, create a new file named `app.py` by clicking `New file` in the `Explorer` tab

![image](https://user-images.githubusercontent.com/64667212/180769043-ffa160c4-d806-421b-aaa8-41dc5087ceaf.png)

Add the code to create your Flask application

```
from flask import Flask, redirect, url_for, request, render_template, session

app = Flask(__name__)
```

The import statement includes references to `Flask`, which is the core of any Flask application. We'll use `render_template` in a while, when we want to return our HTML.

`app` will be our core application. We'll use it when we register our routes in the next step.

### Add the route for our application

Our application will use one route - /. This route is sometimes called either the `default` or the `index` route, because it's the one that will be used if the user doesn't provide anything beyond the name of the domain or server.

Add the following code to the end of `app.py`

```
@app.route('/', methods=['GET'])
def index():
    return render_template('index.html')
```

By using `@app.route`, we indicate the route we want to create. The path will be /, which is the default route. We indicate this will be used for GET. If a GET request comes in for /, Flask will automatically call the function declared immediately below the decorator, `index` in our case. In the body of `index`, we indicate that we'll return an HTML template named `index.html` to the user.


### Create the HTML template for our site

Jinja, the templating engine for Flask, focuses quite heavily on HTML. As a result, we can use all the existing HTML skills and tools we already have. We're going to use Bootstrap to lay out our page, to make it a little prettier. By using Bootstrap, we'll use different CSS classes on our HTML. If you're not familiar with Bootstrap, you can ignore the classes and focus on the HTML (which is really the important part).


Templates for Flask need to be created in a folder named templates, which is fitting. Let's create the folder, the necessary file, and add the HTML.

1. Create a new folder named `templates` by selecting `New Folder` in the `Explorer` tool in Visual Studio Code

2. Select the `templates` folder you created, and select `New File` to add a file to the folder

3. Name the file `index.html`

4. Add the following HTML
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
        integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    <title>Translator</title>
</head>
<body>
    <div class="container">
        <h1>Translation service</h1>
        <div>Enter the text you wish to translate, choose the language, and click Translate!</div>
        <div>
            <form method="POST">
                <div class="form-group">
                    <textarea name="text" cols="20" rows="10" class="form-control"></textarea>
                </div>
                <div class="form-group">
                    <label for="language">Language:</label>
                    <select name="language" class="form-control">
                        <option value="en">English</option>
                        <option value="it">Italian</option>
                        <option value="ja">Japanese</option>
                        <option value="ru">Russian</option>
                        <option value="de">German</option>
                    </select>
                </div>
                <div>
                    <button type="submit" class="btn btn-success">Translate!</button>
                </div>
            </form>
        </div>
    </div>
</body>
</html>
```

The core components in the HTML above are the `textarea` for the text the user wishes to translate, and the dropdown list (`select`), which the user will use to indicate the target language.

### Test the application

With our initial site created, it's time to test it!

- Open your terminal. 

- Run the following command to set the Flask runtime to development, which means that the server will automatically reload with every change:

```
# Windows
set FLASK_ENV=development

# Linux/macOS
export FLASK_ENV=development
```

- Run the application!
```
flask run
```

Open the application in a browser by navigating to http://localhost:5000

You should see the form displayed! Congratulations!


## Create the Translator service

Once the project is up and running, we will create the necessary services on Azure. We'll obtain the necessary keys to call the service, and properly store them in a .env file.

1. Get Translator service key

2. Create .env file to store the key


### Get Translator service key 

- Browse to the [Azure portal](https://portal.azure.com/#home)

- Select `Create a resource`

![image](https://user-images.githubusercontent.com/64667212/180775259-95f3efd7-447d-4a5e-b1d6-a6a11ee209ef.png)

- In the `Search` box, enter `Translator`

- Select `Translator`

![image](https://user-images.githubusercontent.com/64667212/180775398-493a1cc7-db8f-437f-9c08-25470d3e814e.png)

- Select `Create`

![image](https://user-images.githubusercontent.com/64667212/180775498-9e2064fd-cb24-4c85-a95b-6f0422af93e4.png)

- Complete the Create Translator form with the following values:

```
Subscription: Your subscription
Resource group:
Select Create new
Name: flask-ai
Resource group region: Select a region near you
Resource region: Select the same region as above
Name: A unique value, such as ai-yourname
Pricing tier: Free F0 
```

![image](https://user-images.githubusercontent.com/64667212/180775630-4b36339e-2bd6-41bd-a986-c5e0ccd898c9.png)

- Select `Review + create`

- Select `Create`

- After a few moments the resource will be created

- Select `Go to resource`

- Select `Keys and Endpoint` on the left side under `RESOURCE MANAGEMENT`

- Next to `KEY 1`, select `Copy to clipboard`

![image](https://user-images.githubusercontent.com/64667212/180775981-94e57a72-84cc-46b0-9721-6df1eccc0fb4.png)

Note: There's no difference between Key 1 and Key 2. By providing two keys you have the opportunity to migrate to new keys, by regenerating one while using the other.

- Make a note of the Text Translation and location values

### Create .env file to store the key

- Return to Visual Studio Code and create a new file in the root of the application by selecting New file and naming it `.env`

- Paste the following text into .env

```
KEY=your_key
ENDPOINT=your_endpoint
LOCATION=your_location
```
- Replace the placeholders

your_key with the key you copied above
your_endpoint with the endpoint from Azure
your_location with the location from Azure

- our .env file should look like the following (with your values):

```
KEY=00d09299d68548d646c097488f7d9be9
ENDPOINT=https://api.cognitive.microsofttranslator.com/
LOCATION=westus2
```

## Call the service from the app

To close out the workshop, we will add the code to call the Translator service. We'll finish by testing the application and seeing text translated in our app!

1. Add code to call the service

2. Create the template to display results

3. Test our application

### Add code to call the service

app.py contains the logic for our application. We'll add a couple of required imports for the libraries we'll use, followed by the new route to respond to the user.

- At the very top of app.py, add the following lines of code:
```
import requests, os, uuid, json
from dotenv import load_dotenv
load_dotenv()
```
The top line will import libraries that we'll use later, when making the call to the Translator service. We also import `load_dotenv` from `dotenv` and execute the function, which will load the values from .env.

- At the bottom of app.py, add the following lines of code to create the route and logic for translating text:

```
@app.route('/', methods=['POST'])
def index_post():
    # Read the values from the form
    original_text = request.form['text']
    target_language = request.form['language']

    # Load the values from .env
    key = os.environ['KEY']
    endpoint = os.environ['ENDPOINT']
    location = os.environ['LOCATION']

    # Indicate that we want to translate and the API version (3.0) and the target language
    path = '/translate?api-version=3.0'
    # Add the target language parameter
    target_language_parameter = '&to=' + target_language
    # Create the full URL
    constructed_url = endpoint + path + target_language_parameter

    # Set up the header information, which includes our subscription key
    headers = {
        'Ocp-Apim-Subscription-Key': key,
        'Ocp-Apim-Subscription-Region': location,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    # Create the body of the request with the text to be translated
    body = [{ 'text': original_text }]

    # Make the call using post
    translator_request = requests.post(constructed_url, headers=headers, json=body)
    # Retrieve the JSON response
    translator_response = translator_request.json()
    # Retrieve the translation
    translated_text = translator_response[0]['translations'][0]['text']

    # Call render template, passing the translated text,
    # original text, and target language to the template
    return render_template(
        'results.html',
        translated_text=translated_text,
        original_text=original_text,
        target_language=target_language
    )
```

The code is commented to describe the steps that are being taken. At a high level, here's what our code does:

1. Reads the text the user entered and the language they selected on the form
2. Reads the environmental variables we created earlier from our .env file
3. Creates the necessary path to call the Translator service, which includes the target language (the source language is automatically detected)
4. Creates the header information, which includes the key for the Translator service, the location of the service, and an arbitrary ID for the translation
5. Creates the body of the request, which includes the text we want to translate
6. Calls `post` on `requests` to call the Translator service
7. Retrieves the JSON response from the server, which includes the translated text
8. Retrieves the translated text (see the following note)
9. Calls `render_template` to display the response page

### Create the template to display results

Let's create the HTML template for the results page.

- Create a new file in templates by selecting templates in the Explorer tool in Visual Studio Code. Then select New file

- Name the file `results.html`

- Add the following HTML to `results.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
        integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    <title>Result</title>
</head>
<body>
    <div class="container">
        <h2>Results</h2>
        <div>
            <strong>Original text:</strong> {{ original_text }}
        </div>
        <div>
            <strong>Translated text:</strong> {{ translated_text }}
        </div>
        <div>
            <strong>Target language code:</strong> {{ target_language }}
        </div>
        <div>
            <a href="{{ url_for('index') }}">Try another one!</a>
        </div>
    </div>
</body>
</html>
```

You'll notice that we access `original_text`, `translated_text`, and `target_language`, which we passed as named parameters in `render_template` by using `{{ }}`. This operation tells Flask to render the contents as plain text. We're also using `url_for('index')` to create a link back to the default page. While we could, technically, type in the path to the original page, using `url_for` tells Flask to read the path for the function with the name we provide (`index` in this case). If we rearrange our site, the URL generated for the link will always be valid.

### Test our application

- Use Ctrl-C to stop the Flask application

- Execute the command flask run to restart the service

- Browse to http://localhost:5000 to test your application

- Enter text into the text area, choose a language, and select Translate






