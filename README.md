# Documentation




--------------
##This is the final project for the IntroSDE course in UNITN##

####Foreword:
![alt tag](http://ronisweigh.com/wp-content/uploads/2014/02/tired-just-tired-dog-deck-meme.jpg)

--------------

#Architecture:
![alt tag](http://i66.tinypic.com/2qxmcd1.png)
 
#Project:

Consists of 6 Web-services and 1 Console-client:

### 1) Motivational Phrases Service (REST):

To motivate user for achieving their goals, there was made REST-service called "Motivational Phrases Service".
The service pulls quotes from STANDS4 Quotes API(http://www.quotes.net/quotes_api.php).

##### How to use this service:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman)
The service has a single REST method.
* Retrive a random motivational quote: 
  - method: GET
```
GET http://localhost:6912/motivational-phrases/phrase/random
```

### 2) Goal Service (SOAP):
Goal service based on The Beeminder API(https://www.beeminder.com/api/) and it helps users view and manage their goals.

##### Documentation:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman).
Type in the URL field:
```
http://localhost:6915/soap/goals
```

* Get a user's goals: 
  - Must provide:
    - access_token: uniquely identifies a user

* Create a goal for a specific user: 
  - Must provide:
    - access_token: uniquely identifies a user
	  - slug: uniquely identifies a goal
	  - title: the title of the goal
	  - goal_type: depends on what the user wants to achieve (fatloser or gainer)
	  - goaldate: the date the goal should be achieved in unix time format (in seconds)
	  - goalval: the value to be achieved for the goal 
	  - initval: the current value 
	  
* Create a datapoint for a specific goal: 
  - Must provide:
    - access_token: uniquely identifies a user
	- slug: uniquely identifies a goal
	- value: the current value
	- timestamp: the date of the datapoint in unix time format


### 3) Health Management Service (SOAP):
The Health Management Service allows users to store, create, update and retrieve information about their health.

In this case the service focuses on storing the user's information and the user's weight, height and blood pressure.
 
##### Documentation:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman).
Type http://localhost:6911/soap/people in the URL field.
* Read all people in the database

* Read a specific person's data: 
  - Must provide:
    - personId: uniquely identifies a user
    
* Updates a person's credentials: 
  - Must provide:
    - personId: uniquely identifies a user
  - Provide at least one of the following:
	- firstname: user's firstname
	- lastname: user's lastname
	- birthdate: user's birthdate

* Creates a new person: 
  - Must provide:
	- firstname: user's firstname
	- lastname: user's lastname
	- birthdate: user's birthdate
  - measure is not mandatory but if added you must provide, for each:
	- measureType: either weight, height or bloodPressure
	- measureValue: the value of the measure
	- measureValueType: float

* Deletes a person: 
  - Must provide:
	- personId: uniquely identifies a user

* Read a person's history for a specific measure type: 
  - Must provide:
	- personId: uniquely identifies a user
	- measureType: either weight, height or bloodPressure

* Read a person's specific measurement: 
  - Must provide:
	- personId: uniquely identifies a user
	- measureType: either weight, height or bloodPressure
	- mid: uniquely identifies a measurement

* Adds a new measurement for a specific person: 
  - Must provide:
	- personId: uniquely identifies a user
	- measureType: either weight, height or bloodPressure
	- measureValue: the value of the measure

* Returns all measure types saved in the database: 

* Updates a person's measurement: 
  - Must provide:
	- personId: uniquely identifies a user
	- mid: uniquely identifies a measurement
	- dateRegistered: the date in which the measurement was registered
	- measureType: either weight, height or bloodPressure
	- measureValue: the value of the measure
	- measureValueType: float

* Returns all measures of a specific person from/to a specific date: 
  - Must provide:
	- personId: uniquely identifies a user
	- measureType: either weight, height or bloodPressure
	- before: date 
	- after: date

* Returns all people that have a measurement of a specific type in a specific range: 
  - Must provide:
	- measureType: either weight, height or bloodPressure
	- maxValue: value of the maximum measure
	- minValue: value of the minimum measure

### 4) Email Service (REST):
This Service based on The Mandrill Api. The service can send the emails such as confirming a registrations, motivational quotes etc.

##### Documentation:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman).
The service has a single REST method.
* Retrive a random motivational quote: 
  - method: POST
  - media type: text/xml
  - make sure you change:
    - id: the user's health-management_service id
  - make sure to change the email in the body of the request
```
POST http://localhost:6913/mandrill-email/message/welcome/id
```

### 5) Registration service (REST):
The Registration Activation Service allows the VirtualLifeCoach system to retrieve the user's access_token for Goal Service(2).

##### Documentation:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman).
The service has a single REST method.
* Retrive a random motivational quote: 
  - method: GET
  - make sure you change:
    - access_token: the user's beeminder access token
```
GET http://localhost:6914/registration-activation/register?access_token=example
```

### 6) Process centric service (SOAP):
The Process Centric Service connects all the other services.

##### Documentation:
Open an up-to-date web browser(Google Chrome) or some program which allowed to send HTTP requests(Postman).
Type http://localhost:6910/soap/VirtualLifeCoach in the URL field.
* Read a person's information:
  - Must provide:
    - personId: uniquely identifies a user
	- email: the email the user used to register

* Register a new user: 
  - Must provide:
    - email: the user's email
	- firstname: the user's firstname
	- lastname: the user's lastname
	- birthdate: the user's birthdate (yyyy-MM-dd hh:mm:ss)
	- weight: the user's weight
	- height: the user's height
	- bloodpressure: the user's bloodpressure

* Create a new goal for a specific user: 
  - Must provide:
    - personId: the user's unique identifier
	- email: the user's email
	- goaldate: the desired date the goal should be reached
	- type: the goal type, either weight or bloodPressure
	- goalval: the desired value to be reached by the goaldate

* Get all the user's goals: 
  - Must provide:
    - personId: the user's unique identifier
	- email: the user's email

* Add a measure for the specified user: 
  - Must provide:
    - personId: the user's unique identifier
	- email: the user's email
	- type: the measurement type, either weight, height or bloodPressure
	- value: the measurement's new value

# Running the services:
1. Download this repository on your computer.
2. Make 7 different Dynamic Web-Projects in Eclipse or another IDE
2. Add to each service and the client the respective libraries specified in the ivy.xml file.
3. Compile each service and and the client
4. Run each service individually. Run process centric service only after running previous services
5. Run the client and follow the client's instructions


P.S. I worked alone.
