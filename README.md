Parsley
=======

### Summary ###
Parsley is a personalized restaurant recommendation engine that solves the age-old question – where should I go to eat? It addresses a gap in today’s recommendation engines landscape, using cosine similarities as a quick, simple way to understand a user's preferences and make tailored recommendations.

Parsley searches more than 1.1 million US restaurants (with more than 40 restaurant-specific attributes for each) to surface relevant suggestions to users based on restaurants, bars and cafes that they already love – then allows users an easy way to store, access and visualize their favorite restaurants. 


### Details ###
- [Stack](#stack)
- [How it works](#how-it-works)
- [Features](#features)
- [Screenshots](#screenshots) 
- [Cosine similarity](#cosine-similarity)
- [Installation](#install)
- [Acknowledgements](#acknowledgements)

### Stack ###
•	Python 2.7 <br>
•	Factual API 1.6.0 <br>
•	Flask 0.9<br>
• Flask-Login 0.2.11 <br>
• Flash-WTF 0.8 <br>
•	Flask-SQLAlchemy 0.16<br>
• GeoJSON 1.0.9 <br>
•	Google Maps JS embed API  <br>
•	Jinja2 2.6<br>
•	Mapbox JS API (Leaflet JS) <br>
• Numpy 1.9.1 <br>
•	OAuth 1.0.1 <br>
• OAuthlib 0.7.2 <br>
•	Passlib <br>
•	Scikit-learn 0.15.2 <br>
• Scipy 0.14.0 <br>
• SQLAlchemy 0.7.9 <br>
•	Werkzeug 0.8.3<br>
•	Wsgiref 0.1.2<br>
•	WTForms 1.0.2<br>


### How it works ###
•	Users provide feedback on suggestions as Parsley learns their preferences, using sci-kit learn's cosine similarity algorithm to find restaurants with feature vectors that closely match a user’s preferences for a requested zipcode and cuisine. 

Learn more about how cosine similarity is calculated <a href =#cosine-similarity> below</a>. 

### Features ###
1.	Secure user log in functionality (pbkdf2_sha256 encryption) <br>
2.	Search and autocomplete functionality (Google Maps JS Autocomplete)<br>
3.	Matching/recommendation algorithm (using scikit-learn) <br>
4.	Filter by zip code and cuisine <br>
5.	Save and view user favorites (LeafletJS/Mapbox JS API)<br>
 
### Screenshots ###

#### <strong> Landing page</strong><br><br>
![image](/screenshots/splash.png)<br><br>

#### <strong>Login page</strong><br><br>
![image](/screenshots/login.png) <br><br>

Users sign up and log in using secure pbkdf2_sha256 encryption (assuming https connection). After signup, users complete a quick preferences survey that inserts a row into the user_preferences SQLite table, which has a one-to-one relationship with the user table.

#### <strong>Restaurant search</strong><br><br>
![image](/screenshots/restaurants_page.png) 
<strong> Restaurant suggestions: </strong> A user can enter three restaurants that he or she likes, along with the zip code that they would like to search, and receive tailored recommendations, with the option to filter by either cuisine or category (bar, coffee shop, etc.).
<br><br>
<strong>Google autocomplete: </strong>
When a user starts to type in restaurants, Google Maps’ Javascript API autocompletes the text entry to ensure submission format is standardized to be restaurant, address, city, state, country.
If a user clicks submit with incorrectly formatted information, they’ll see an error page and be returned to the form to re-enter information.
<br> <br>
Upon clicking “submit”, a call to the server is made to check whether these restaurants exist in the Parsley database yet. If not, Parsley makes an API call to Factual (after parsing the entry to determine name, city, etc.) to search for restaurants by that name in that city. If restaurants are found, Parsley then takes the JSON object from Factual and inserts appropriate information into the restaurant, restaurant_features and user_restaurant_rating table. 
<br>

#### <strong> Results </strong> <br><br>
![image](/screenshots/all_cuisines.png) <br>
<strong> Suggest new restaurants: </strong><br><br>
Given that information, Parsley now loops over all restaurants a user has ever rated positively, along with a user’s initial preferences, to create a comprehensive dictionary of weighted user preferences. <br><br>
1.	Parsley then identifies the top 3 features that are important to a user – and queries the restaurant table for
restaurants that contain these features in the zip code that the user specified and creates a dictionary of all entries.
<br><br>
2. Using scikit learning’s DictVectorizer, both the user preferences and restaurant results dictionary are converted to vectors.<br><br>
3. Using scikit learning’s cosine similarity algorithm, Parsley then ranks all restaurants with those top features in that zip code by which restaurants most closely match a user’s preferences. 
<br><br>
#### <strong> Favorites </strong> <br><br>
![image](/screenshots/favorites.png)<br><br>
Parsley returns ranked restaurants to the user on a feedback page, which contains restaurant contact information and the ability to upvote/downvote restaurants.
<br>

#### <strong> Browse </strong> <br><br>
![image](/screenshots/browse.png)<br><br>
An alternative path for the user to quickly view suggested restaurants using their default zipcode established at signup. Users can modify their initial set of preferences and demographic information at any time by clicking on the My Profile tab.
<br>

### Cosine similarity ###
 
Cosine similarity measures the similarity between two vectors of an inner product space that measures the cosine of the angle between them. The cosine of 0° is 1, and it is less than 1 for any other angle. <br>

It is thus a judgement of orientation and not magnitude: <br>

•	Two vectors with the same orientation have a Cosine similarity of 1 <br>
•	Two vectors at 90° have a similarity of 0 <br>
•	Two vectors diametrically opposed have a similarity of -1 <br>

Cosine similarity is particularly used in positive space, where the outcome is neatly bounded in [0,1].

Note that these bounds apply for any number of dimensions, and Cosine similarity is most commonly used in high-dimensional positive spaces. For example, in Information Retrieval and text mining, each term is notionally assigned a different dimension and a document is characterised by a vector where the value of each dimension corresponds to the number of times that term appears in the document. 

Cosine similarity then gives a useful measure of how similar two restaurants are likely to be in terms of their subject matter – specifically, their features.

### Installation <br><br>

1. Clone this repo and install all the required libraries.

  ```
  git clone https://github.com/laurenchow/Parsley/
  ```
2. Create a python virtual environment:

  ```
  virtualenv env
  ```
3. Activate the virtual environment:

  ```
  source env/bin/activate
  ```
4. Install the requirements:

  ```
  pip install -r requirements.txt
  ```
5. Sign up for API access keys for Factual, Mapbox and Google Maps and store the API key for Factual and Mapbox in your environment.

6. Open model.py by typing:
  ```
  python model.py
  ```
  into your terminal. 

7.  After model.py runs successfully, run python parsley.py, then point your browser to http://localhost:5000/ and get started!
 

### Acknowledgements 
Image credit:<br>
• Leaf by Evan MacDonald and emoticons (Happy, Sad)  by Paul F. from The Noun Project from <a href = "https://www.http://thenounproject.com/"> The Noun Project</a><br>
• Background images from <a href = "https://unsplash.com/grid"> Unsplash
 
### Sources: <br>
http://en.wikipedia.org/wiki/Collaborative_filtering#Model-based <br>
http://en.wikipedia.org/wiki/Cosine_similarity<br>
http://www.cs.carleton.edu/cs_comps/0607/recommend/recommender/itembased.html

 
