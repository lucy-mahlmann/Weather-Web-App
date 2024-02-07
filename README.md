# Texas Cities Weather Web Application

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-application">About The Application</a></li>
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

Running Application Locally
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
- Used python bcrypt library to encrypt and decrypt admin and user passwords
    * Only stores the encrypted password in the table
TODO say where this code is from 
```
encrypted_password = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())
    newuser = User(name=name, password=encrypted_password)
```


<!-- DEPLOYMENT -->
## Deployment

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


