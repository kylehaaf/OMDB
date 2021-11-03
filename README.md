# OMDB
Django search app challenge

This is a simple, single database table application for storing titles.
The application also uses the Open Movie Database API to look up the titles which are not available locally.

### IMPORTANT 
The original requirement is to include a search for an episode type as well. Playing around with the OMDB API, it didn't return any episode results and series type titles don't include any objects linking either to seasons or episodes.
I believe, the episode search type is there to implement local database table dependency for episodes being linked to the series.
However, this has not been implemented due to above-mentioned issue.

### How it works?
Generally, the application first searches for the title in the local database.
If there are titles containing the searched key word, they will be displayed.
If there are no such titles available, the application calls the OMDB API.
If the title is available there, it gets stored to the local database and displayed on the screen.

### The search behaviour
* Clicking on the Search button without any keyword or title type returns all titles in the local database.
+ Searching with just a title name returns all titles from the local database containing the keyword.
+ If no such titles exist, the app calls the OMDB API, stores the result locally and displays the result on the screen.
* Search by title type returns all titles of the selected type.

### Downside of this app
* Combination of a title name and title type still returns all relevant titles based on the search word. The type doesn't play any role.
* It's not possible to get additional titles from a franchise if similar titles already exist.
Example:
* The local database already contains movies Blade II, Blade Runner and Blade Runner 2049.
* Searching for the first Blade movie always returns those movies and doesn't search OMDB for the actual title.

### How to run the app locally
Install virtual environment:
* Pipenv has been used to run the app
virtual environment runs *pipenv*, version 2021.5.29
*brew install pipenv*

* In the terminal, navigate to the project folder.
* Run  *pipenv shell*  - this will start the virtual environment

These are the requirements to get the app up and running:

> python==3.9.6 \
> django==3.2.8 \
> requests==2.26.0

Commands: \
*pip install python==3.9.6* \
*pip install django==3.2.8* \
*pip install requests==2.26.0*

Check in the packages whether the following ones are already included after installing Django.
If not, they need to be added too.
> asgiref==3.4.1 \
> certifi 2021.10.8 \
> charset-normalizer==2.0.7 \
> idna==3.3 \
> pytz==2021.3 \
> sqlparse==0.4.2 \
> urllib3==1.26.7

* Run *python manage.py runserver* - this will start the actual application

### Testing
Available endpoints
* Home " / "
* Preview of found titles " /result "
* Title details page " /result/<title_id> "

#### The happy path
| Action | Result |
|--------|--------|
|click on Search|All locally stored titles should be displayed|
|select a type and click on Search|Only the specific type of titles - (movies or series) - should be returned|
|type a title name which is already in local database|all titles containing the search word should be returned|
|type a title name and select a type|all titles containing the search word should be returned|
|type a title name which is not in the local database|it should display the result incl. a message saying the title has been searched on OMDB and saved to our local database|
|type a title name which is not in the local database, select a title type as well|it should display the result incl. a message saying the title has been searched on OMDB and saved to our local database|
|click on the image on each result|the detail page should appear with the title details|
|while on the detail page, change the ID - use another existing one|It should display the other title details|

#### The sad path - error handling
| Action | Result |
|--------|--------|
|while still on the detail page, change the ID - use a non existing integer|this should redirect the user to the home page and display an error message|
|while still on the detail page, change the ID - use a random string/character|this should a Not found 404 page|
|refresh the app so you are back on the home page. Add *result* to the URL|this should redirect the user back to the home page and display an error message|
|use a wrong/misspelled title name - example *ong bag*|this should redirect the user back to the home page and display an error message|
|disconnect from the internet and search for a title which is not in the local database to trigger the OMDB call|this should redirect the user back to the home page and display an error message|