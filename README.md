# Milestone 2

## Description of Project

Ash's Movie Explorer is a web application that randomizes a movie and its details everytime the page is reloaded. The details (title, tagline, genres, movie poster image, and Wikipedia article link) are dynamically fetched using the TMDB and Wikipedia API. The web application is served via the flask framework, and deployed on Heroku. In addition, I added a login/register page that allows a user to login and register for the movie explorer. Lastly, there is a new section on every movie allowing for an addition of ratings and comments. 

## Layout of Project

The contents of this web application include "static" and "templates" directories. The CSS components (style.css and external images) are found within the static directory. The templated directory now contains 3 HTML pages: login.html, register.html, index.html. In addition, the project directory includes app.py, tmdb.py, a Procfile, requirements.txt, a .env file, and a .gitignore file. The flask framework is running in app.py, whereas the API fetching is done in my api.py file. In addition, we use flask SQLALCHEMY in order to connect to our Heroku Database. 

### Using Flask

Within this file I imported these libraries: os, random, flask, requests, and tmdb (get_title, get_tagline, get_genre, get_image, get_wiki_page). 
* I used the os library to help hide my API key in a .env file. 
* I used random to help pick a random movie id from the list. 
* I used flask to help create the web application, route it to the correct place, and connect it to the HTML page. 
* The requests library allows us to send HTTP requests using python. 
* I also needed to import all the methods written for API calls into app.py so they could be easily called and rendered to the HTML file. 
* I used flask_sqlalchemy to get SQLAlchemy, the extension for flask that makes it simple to do commmon tasks with a database in python. 
* I used flask_login and its methods in order to provide user session management for my project. 

I have two methods in this file: get_random_movie and get_movie. get_random_movie randomizes the movie id and builds the movie base url and configures the json. It is then called in get_movie (the routed function). In this function, we return flask.render_template which sends all my method calls to the HTML file. We then call app.run and include a host and a port so it can be deployed in Heroku.

### API Fetching

Within this file I imported these libraries: os, requests, dotenv (load_dotenv, find_dotenv). 
* I used the os library to help hide my API key in a .env file. 
* I used the dotenv library to call the methods listed above to help recognize my API key from the .env file, so the file will call the APIs as intended. 
* The requests library allows us to send HTTP requests using python. 

I created a separate function for each piece of information we were fetching for the TMDB Movie API. This resulted in 4 methods: get_title, get_tagline, get_genre, get_image. Using the movie json response (made in app.py), we can easily filter through the JSON to find the title and tagline. The genres were a bit more complicated because the output was a list of dictionaries, so it needed to be looped through and picked the value from the key-value pair. For the movie poster, I had to use the configuration API and build the final url from the base_url + size + poster_path. 

The last function in this file is get_wiki_page (calls information from Wiki API). I constructed the Wikipedia article url from the link and the movie title that was passed through as a query parameter from the get_title function above. 

### Heroku
The application is deployed on Heroku. We created a heroku database in order to store information for the application. The application can be found at: https://vast-forest-34825.herokuapp.com/

## Main Changes in Milestone 2

### Login & Register
I created 2 HTML pages each with its own HTML form to login and register for the movie explorer. In app.py, I created corresponding app.route functions. The login page is the default page, and the user is asked if they want to login or register. If an invalid username is entered, they are given a message and redirected to the register page (using flask.redirect).
The register page allows for a user to enter their name and if it is not taken, adds it to the database and redirects them to the movie page. 

### Ratings/Comments
Likewise, there is a form in index.html prompting the user for their rating and commments on a movie. Included is a readonly movie id variable that lets the code know to what movie the ratings are connected to in the database. A rating and comment is added to the database and redirected to the movie page in which the movie id is queried and passed through the render_template function. 

### Database 
I created two separate tables: a User table (for storing username information), and a Comment table (for storing ratings, reviews, current username, and movie id). 

## Answered Questions

### How to run locally?

#### Install general libraries
1. pip install python-dotenv
2. pip install requests
3. pip install flask
#### PostgreSQL Setup
5. brew install postgresql (if on mac)
6. brew services start postgresql (if on mac)
7. psql -h localhost  # this is just to test out that postgresql is installed okay - type "\q" to quit
8. pip install psycopg2-binary
9. pip install Flask-SQLAlchemy==2.1
#### Heroku Setup
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)  # install Homebrew
brew tap heroku/brew && brew install heroku  # install Heroku CLI

### What are 3 technical issues you encountered with your project? How did you fix them?

1. The biggest issue I had was circular imports. I initially had made 2 separate python files - one for the TMDB API and one for the Wiki API. I originially passed through movie id to every function made, however that caused me to have to import app.py into both files, and both files were already imported into app.py in order to call the functions. This caused a circular import error so I ended up moving the Wiki API function to tmdb.py and created a global "movies" variable that was the JSON response. 
2. I struggled to fetch just the genres from the API. The output was a list of dictionaries, so I had to research how to only get the values from each dictionary. I ended up looping through the list and adding the values to a string separated by commas. 
3. When creating the Wiki API function, I struggled on setting up the final article url. The Wiki API was much harder to understand and read than the TMDB one for me. At first I tried using the Info API to pull the full url from it. However, I needed the Wiki movie id, which I did not know how to get. I ended up creating a global movie JSON response variable, and was able to pass that through the function. Because of this variable, the Wiki API now knows what movie to search for.  

### What are known problems, if any, with your project?

My project meets all the requirements!

### What would you do to improve your project in the future?

By talking to some classmates, I realized that it would have been much easier to have 2 separate functions to get the movie data and to get the Wikipedia data. I did not realize in python that you are able to return multiple variables in one line. Because of this assumption, I separated my details into multiple functions. 
