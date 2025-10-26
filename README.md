
# Rest Booking API Testing with Postman Newman

This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the API.




## Features

- Tests for GET, POST, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations


## API Documentation

- https://documenter.getpostman.com/view/40075274/2sB3Wk14eT

# Technology used:
- Postman
- Newman

# Prerequisite:
- Node js
- Newman
- Newman Html Report Library








## Installation

1. Postman: If you haven't already, download and install Postman.
2. Clone the repository:
```bash
git clone https://github.com/MysaraNur/restful-booker-api-testing
```

3. Import the Postman collection:
- Open Postman.
- Click on the Import button.
- Select the file from the repository.
4. Import the Postman environment:
- In Postman, click on the gear icon in the top right corner.
- Select Import and choose the file.
5. Newman and Report Installation Process:
- Newman Install Command:
```bash
 npm install -g newman
 ```
 - Newman Html Report Install Command:
```bash
 npm install -g newman-reporter-htmlextra
 ```
 
## Usage

1. Select Environment:
- In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
2. Run Collection:
- Select the imported collection from the Collections sidebar.
- Click on the Runner button to open the collection runner.
- Select the desired environment.
- Click Start Test to run the collection.
3. View Results:
- Once the tests are complete, view the results in the Runner tab.
- Detailed test results can be viewed for each request.

## Testing 
# Test Case Scenarios:
# 1. Create New Booking

- Request URL: https://restful-booker.herokuapp.com/booking/

- Request Method: POST

# Pre-request Script:
```javascript 
var firstName = pm.variables.replaceIn("{{$randomFirstName}}");
console.log("First Name:", firstName);
pm.environment.set("firstName", firstName);


var lastName = pm.variables.replaceIn("{{$randomLastName}}");
console.log("Last Name:", lastName);
pm.environment.set("lastName", lastName);


var totalprice = parseInt(pm.variables.replaceIn("{{$randomInt}}"));
console.log("Total Price:", totalprice);
pm.environment.set("totalprice", totalprice);


var depositpaid = Math.random() < 0.5; // Random true or false
console.log("Deposit Paid:", depositpaid);
pm.environment.set("depositpaid", depositpaid);


const moment = require('moment');
var checkIn = moment().add(1, 'months').add(5, 'days').format("YYYY-MM-DD");
console.log("Check-in Date:", checkIn);
pm.environment.set("checkIn", checkIn);


var additionalneeds = ["Breakfast", "Lunch", "Dinner", "Airport Pickup"];
var randomNeed = additionalneeds[Math.floor(Math.random() * additionalneeds.length)];
console.log("Additional Needs:", randomNeed);
pm.environment.set("additionalneeds", randomNeed);
```
# Request Body:

```javascript
{
    "firstname": "{{firstName}}",
    "lastname": "{{lastName}}",
    "totalprice": {{totalprice}},
    "depositpaid": {{depositpaid}},
    "bookingdates": {
        "checkin": "{{checkIn}}",
        "checkout": "{{checkOut}}"
    },
    "additionalneeds": "{{additionalneeds}}"
}
```
## 2. Get Booking Details By ID
- Request URL: https://restful-booker.herokuapp.com/booking/bookingid
 - Request Method: GET
 Response Body:
```javascript
{
   "firstname": "D'angelo",
   "lastname": "Feeney",
   "totalprice": 757,
   "depositpaid": true,
   "bookingdates": {
       "checkin": "2024-03-15",
       "checkout": "2024-03-20"
   },
   "additionalneeds": "hard drive"
}
```
## 3. Create A Token For Authentication.
- Request URL: https://restful-booker.herokuapp.com/auth
 - Request Method: POST
 - Pre-request Script: None
 Request Body:
```javascript
{
   "username": "admin",
   "password": "password123"
}
```
Response Body:
```javascript
{
   "token": "06eb798bf6f2caa"
}
```
## 4. Update the Booking Details
- Request URL: https://restful-booker.herokuapp.com/booking/bookingid
- Request Method: PUT
# Pre-request Script:
```javascript
var jsonData = pm.response.json();

pm.test("First Name validation", function() {
    pm.expect(jsonData.firstname).to.eql(pm.environment.get("Updated_firstName"));
});

pm.test("Last Name validation", function() {
    pm.expect(jsonData.lastname).to.eql(pm.environment.get("Updated_lastName"));
});

pm.test("Verify total price validation", function() {
    pm.expect(jsonData.totalprice).to.eql(pm.environment.get("Updated_totalprice"));
});

pm.test("Verify deposit paid validation", function() {
    pm.expect(jsonData.depositpaid).to.eql(pm.environment.get("Updated_depositPaid"));
});

pm.test("Verify check-in validation", function() {
    pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get("Updated_checkIn"));
});

pm.test("Verify check-out validation", function() {
    pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get("Updated_checkOut"));
});

pm.test("Verify additional needs validation", function() {
    pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get("Updated_additionalNeeds"));
});


```
# Request Body:

```javascript 
{
	"firstname": "{{Updated_firstName}}",
	"lastname": "{{Updated_lastName}}",
	"totalprice": "{{Updated_totalprice}}",
	"depositpaid": "{{Updated_depositPaid}}",
	"bookingdates": {
    	"checkin": "{{Updated_checkIn}}",
    	"checkout": "{{Updated_checkOut}}"
	},
	"additionalneeds": "{{Updated_additionalNeeds}}"
}
```
# Response Body:

```javascript 
 {
     "bookingid": 4334,
     "booking": {
         "firstname": "Joelle",
         "lastname": "Krajcik",
         "totalprice": 266,
         "depositpaid": true,
         "bookingdates": {
             "checkin": "2024-03-15",
             "checkout": "2024-03-20"
         },
         "additionalneeds": "monitor"
     }
 }
 ```
 ## 5. Delete Booking Record
 - Request URL: https://restful-booker.herokuapp.com/booking/bookingid
 - Request Method: DELETE
- Response Body: None
# Run Command:
- Run Command for Console:
```bash
newman run Restful_Booker.postman_collection.json -e RestfulBooker.postman_environment.json 
```
- Run Command for Report:
```bash
newman run Restful_Booker.postman_collection.json -e RestfulBooker.postman_environment.json -r cli,htmlextra
```









## Newman Report Summary:

![Newman report summary](https://drive.google.com/drive/folders/10hOzdmIBYUkwy5R251vzKEN-jM1jJSyV)



