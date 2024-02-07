# Texas Cities Weather Web Application

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-application">About The Application</a></li>
    <li><a href="#running-application-locally">Running Application Locally</a></li>
    <li><a href="#frontend">Frontend</a></li>
    <li><a href="#backend">Backend</a></li>
    <li><a href="#deployment">Deployment</a></li>
    <li><a href="#learning-challenges">Learning Challenges</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

<!-- ABOUT THE APPLICATION -->
## About The Application

A personal weather portal is a web app that allows users to discover and plot historical values of weather parameters for 
different cities in Texas. This web app uses the data made available by the [National Centers for Environmental Information](https://www.ncei.noaa.gov/metadata/geoportal/rest/metadata/item/gov.noaa.ncdc:C00861/html)
You can find the data used [here](https://www.ncei.noaa.gov/pub/data/ghcn/daily/) 

This web app displays four weather parameters: 
- Maximum Temperature
- Minimum Temperature
- Precipitation
- Snow

<!-- RUNNING APPLICATION LOCALLY -->
## Running Application Locally
<ol> 
  <li>python3 -m venv venv</li>
  <li>source venv/bin/activate (or venv\Scripts\activate for WindowsOS) </li>
  <li>For Google OAuth:</li>
    <ul>
       <li>pip3 install --only-binary :all: greenlet</li>
       <li>pip3 install --only-binary :all: pyopenssl</li>
    </ul>
  <li>pip3 install -r requirements.txt</li>
  <li>python3 application.py</li>
</ol>

Try
1. Open application url: 
   - Non ssl: [http://localhost:5009/](http://localhost:5009/)
   - Ssl: [https://localhost:5009/](https://localhost:5009/)
2. Use REST API to (in terminal):
   - register admin
   - register users
   - register cities
3. Access the web app in the browser to test OAuth integration
4. Logout
5. Login again using same user.

Stop 
1. Ctrl+C in the window where application.py was started

<!-- FRONTEND -->
## Frontend

- Implemented with HTML/CSS and JavaScript

User Login Page

<img
  src="/static/images/login_screen.png"
  style="display: inline-block; margin: 0 auto; width: auto; height: auto">
  
Only Allows Users Added by Admin to Login In

<img
  src="/static/images/user_not_found.png"
  style="display: inline-block; margin: 0 auto; width: auto; height: auto">

- Forms for users to register cities with specific weather parameters from a certain year and month
- Implements data checks 
    * City name should not be empty
    * Month should be a 2 digit number
    * At least one of the weather parameters should be selected
  
<img
  src="/static/images/input_checks.png"
  style="display: inline-block; margin: 0 auto; width: auto; height: auto">
  
Selected Registered City Graphs

<img
  src="/static/images/Austin_graphs.png"
  style="display: inline-block; margin: 0 auto; width: auto; height: auto">
  


<!-- BACKEND -->
## Backend
Features
* Encrypted password protection
* Oauth protocol for users to login via email
* User and Admin REST APIs for managing registered users and cities
* ETL process to load weather data into database
* SQLite database to store weather data for registered Texas cities, added Users, and Admins
* Error checking

- Used python ```bcrypt``` library to encrypt and decrypt admin and user passwords
    * Only stores the encrypted password in the registered ```User``` table
```
// password encryption, application.py line 400
encrypted_password = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())
newuser = User(name=name, password=encrypted_password)
```
- Implemented Oauth protocol allows users to login with existing identies through Identity Providers (IDP) as part of this web application. Users can register as a user through Google using a preexisting email.

- User and Admin REST APIs include:
  * POST, GET, DELETE for admin users
  * POST, GET, DELETE for non-admin users
  * POST, GET, DELETE for cities registered by admin
  * POST, GET for user cities

- The web application runs the ETL (Extract, Tansform, and Load) process periodically in the background of the application in order to request the data stored at the [National Centers for Environmental Information](https://www.ncei.noaa.gov/pub/data/ghcn/daily/) website into the ```/data``` directory
- Transformation of the data is done when the user requests a graph of the city weather parameters in the function defined as ```city_status_graph()```
```
// Transformation of data into usuable values by application, application.py lines 677-690
 weather_params = usercity.weather_params
    individual_params = weather_params.split(',')
    for param in individual_params:
        param = param.strip()
        year_month_param = year + "-" + month + "-" + param
        param_values = dbsession.query(WeatherParameter).filter_by(cityId=city.id, year_month_param=year_month_param)
        if param_values.count() > 0:
            param_value_str = param_values.first().values
            op[year_month_param] = param_value_str
        else:
            op[year_month_param] = "" 

    app.logger.info(op)
    return json.dumps(op)
```

- The web application provides a SQLite database that provides five database tables:
  * ```Admin```: lists all added Admins and cities registered by that admin
  * ```User```: lists all users and their encrypted passwords
  * ```City```: lists registered cities and the id of the admin they are registered under
  * ```UserCity```: lists cities that have been added by a user with the specified date and weather parameters; lists id of the user they where added by and the corresponding city id
  * ```WeatherParameter```: lists all weather data that is extracted from registered cities and their corresponding city id
- These database tables are interacted with and manipulated by query calls

- Error checks for users not registered, cities not added by the admin, user provides incorrect password 

<!-- DEPLOYMENT -->
## Deployment

Deployed web app on Amazon AWS using an EC2 instance (VM) 

app: [https://ec2-3-128-179-175.us-east-2.compute.amazonaws.com:5009/](https://ec2-3-128-179-175.us-east-2.compute.amazonaws.com:5009/])  (no longer available)

<!-- LEARNING CHALLENGES -->
## Learning Challenges

comments: 
- I just manually added the /data directory due to the error with the ETL not being able to create one 
  due to using WindowsOS

<!-- CONTACT -->
## Contact
Built as course work for CS 378 (Modern Web Applications) at the University of Texas at Austin

Lucy Mahlmann - lmahlmann@utexas.edu

Project Link: [https://github.com/lucy-mahlmann/Weather-Web-App](https://github.com/lucy-mahlmann/Weather-Web-App)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


