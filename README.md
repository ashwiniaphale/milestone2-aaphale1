# project1-aaphale1

## Description of Project

Ash's Movie Explorer is a web application that randomizes a movie and its details everytime the page is reloaded. The details (title, tagline, genres, movie poster image, and Wikipedia article link) are dynamically fetched using the TMDB and Wikipedia API. The web application is served via the flask framework, and deployed on Heroku. 

## Layout of Project

The contents of this web application include "static" and "templates" directories. The CSS components (style.css and external images) are found within the static directory. The HTML page (index.html) is found within the templates directory. In addition, the project directory includes app.py, tmdb.py, a Procfile, requirements.txt, a .env file, and a .gitignore file. The flask framework is running in app.py, whereas the API fetching is done in my tmdb.py file. 

### Using Flask

Within this file I imported these libraries: os, random, flask, requests, and tmdb (get_title, get_tagline, get_genre, get_image, get_wiki_page). 
* I used the os library to help hide my API key in a .env file. 
* I used random to help pick a random movie id from the list. 
* I used flask to help create the web application, route it to the correct place, and connect it to the HTML page. 
* The requests library allows us to send HTTP requests using python. 
* I also needed to import all the methods written for API calls into app.py so they could be easily called and rendered to the HTML file. 

I have two methods in this file: get_random_movie and get_movie. get_random_movie randomizes the movie id and builds the movie base url and configures the json. It is then called in get_movie (the routed function). In this function, we return flask.render_template which sends all my method calls to the HTML file. We then call app.run and include a host and a port so it can be deployed in Heroku.


### API Fetching

Within this file I imported these libraries: os, requests, dotenv (load_dotenv, find_dotenv). 
* I used the os library to help hide my API key in a .env file. 
* I used the dotenv library to call the methods listed above to help recognize my API key from the .env file, so the file will call the APIs as intended. 
* The requests library allows us to send HTTP requests using python. 

I created a separate function for each piece of information we were fetching for the TMDB Movie API. This resulted in 4 methods: get_title, get_tagline, get_genre, get_image. Using the movie json response (made in app.py), we can easily filter through the JSON to find the title and tagline. The genres were a bit more complicated because the output was a list of dictionaries, so it needed to be looped through and picked the value from the key-value pair. For the movie poster, I had to use the configuration API and build the final url from the base_url + size + poster_path. 

The last function in this file is get_wiki_page (calls information from Wiki API). I constructed the Wikipedia article url from the link and the movie title that was passed through as a query parameter from the get_title function above. 

### Heroku
The application is deployed on Heroku. The application can be found at: https://mysterious-bastion-78769.herokuapp.com/

## Answered Questions

### What happens when someone forks from the repo?

Forking a repo means copying the repo and allowing for free changes without affecting the original project. If one cloned the repo, they would have to have all the frameworks installed in order to run the project. For example if someone cloned my repo, they would have to install flask, requests. They would also need their own API key from TMDB, and to put that within a .env file. 
1. pip install python-dotenv
2. pip install requests
3. pip install flask

### What are 3 technical issues you encountered with your project? How did you fix them?

1. The biggest issue I had was circular imports. I initially had made 2 separate python files - one for the TMDB API and one for the Wiki API. I originially passed through movie id to every function made, however that caused me to have to import app.py into both files, and both files were already imported into app.py in order to call the functions. This caused a circular import error so I ended up moving the Wiki API function to tmdb.py and created a global "movies" variable that was the JSON response. 
2. I struggled to fetch just the genres from the API. The output was a list of dictionaries, so I had to research how to only get the values from each dictionary. I ended up looping through the list and adding the values to a string separated by commas. 
3. When creating the Wiki API function, I struggled on setting up the final article url. The Wiki API was much harder to understand and read than the TMDB one for me. At first I tried using the Info API to pull the full url from it. However, I needed the Wiki movie id, which I did not know how to get. I ended up creating a global movie JSON response variable, and was able to pass that through the function. Because of this variable, the Wiki API now knows what movie to search for.  

### What are known problems, if any, with your project?

My project meets all the requirements!

### What would you do to improve your project in the future?

By talking to some classmates, I realized that it would have been much easier to have 2 separate functions to get the movie data and to get the Wikipedia data. I did not realize in python that you are able to return multiple variables in one line. Because of this assumption, I separated my details into multiple functions. 
