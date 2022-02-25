# Milestone 2 

## Description of Project

Ash's Movie Explorer (Milestone 2) is a web application that randomizes a movie and its details everytime the page is reloaded. The application now includes a login and register page, along with a rating/review section. 

## Layout of Project

The project now contains 2 more HTML files within my templates directory - login.html and register.html. 


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
