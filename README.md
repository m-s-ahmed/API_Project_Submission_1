***Booking related API Testing with Postman Newman***
This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the API. 

***Features***
- Tests for POST, GET, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

***API Documentation:***
- https://documenter.getpostman.com/view/38815293/2sAYJ4jgYH

***Technology used:***
- Postman
- Newman

***Prerequisite:***
- Node Js
- Newman
- Newman Html Report Library

***Installation***

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/m-s-ahmed/API_Project_Submission_1
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
***Usage***

1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
2. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
3. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

***Testing***

## Test Case Scenarios:

  _**1. Create New Booking**_

### base_url:https://restful-booker.herokuapp.com
### Request URL: {{base_url}}/booking/
### Request Method: POST
### Pre-request Script:
```console 
   //Randomly generate first name
var firstName=pm.variables.replaceIn("{{$randomFirstName}}");
pm.environment.set("First_Name",firstName);

//Randomly generate last name
var lastName=pm.variables.replaceIn("{{$randomLastName}}");
console.log(lastName);
pm.environment.set("Last_Name",lastName);

//Randomly generate bookindates
var checkIn=pm.variables.replaceIn("{{$randomDateFuture}}");
console.log(checkIn);//not our desired format,so following next procedure
//console.log(checkIn.format("YYYY-MM-DD")); showing error

//Having a moment library which helps us in time management
const moment=require('moment');
const today=moment();
console.log(today.format("YYYY-MM-DD"));

//Future date
//console.log(today.add(2,'d').format("YYYY-MM-DD")); 
var checkInn=today.add(2,'d').format("YYYY-MM-DD");
pm.environment.set("Check_In",checkInn);

//Generating Checkout date randomly
var checkOut=today.add(2,'d').format("YYYY-MM-DD");
console.log(checkOut);
pm.environment.set("Check_Out",checkOut);

//Randomly generate total price
var totalPrice=pm.variables.replaceIn("{{$randomInt}}");
console.log(totalPrice);
pm.environment.set("Total_Price",totalPrice);

//Randomly generate additional needs
var additionalNeeds=pm.variables.replaceIn("{{$randomLoremWords}}");
console.log(additionalNeeds);
pm.environment.set("Additional_Item",additionalNeeds);

```

  **Request Body:** 
 ```console 
{
	"firstname" : "{{First_Name}}",
	"lastname" : "{{Last_Name}}",
	"totalprice" : {{Total_Price}},
	"depositpaid" : true,
	"bookingdates" : {
    	"checkin" : "{{Check_In}}",
    	"checkout" : "{{Check_Out}}"
	},
	"additionalneeds" : "{{Additional_Item}}"
}

```
  **Response Body:**
 ```console 
{
    "bookingid": 2256,
    "booking": {
        "firstname": "Cale",
        "lastname": "Harris",
        "totalprice": 440,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2024-12-28",
            "checkout": "2024-12-30"
        },
        "additionalneeds": "neque voluptas totam"
    }
}
```

_**2. Get Booking Details By ID**_

### base_url:https://restful-booker.herokuapp.com
### Request URL:{{base_url}}/booking/{{ID}}
### Request Method: GET
### Response Body:
 ```console 
{
    "firstname": "Cale",
    "lastname": "Harris",
    "totalprice": 440,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-12-28",
        "checkout": "2024-12-30"
    },
    "additionalneeds": "neque voluptas totam"
}
```

_**3. Create A Token For Authentication.**_

### base_url:https://restful-booker.herokuapp.com
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
     "token": "fcd054dfbba1f50"
}
```

_**4. Update the Booking Details**_

### base_url:https://restful-booker.herokuapp.com
### Request URL:{{base_url}}/booking/{{ID}}
### Request Method: PUT
### Pre-request Script:
```console 
    var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
    pm.environment.set("firstName", firstName)
    console.log("First Name Value "+firstName)
    
    var lastName = pm.variables.replaceIn("{{$randomLastName}}")
    pm.environment.set("lastName", lastName)
    console.log("Last Name Value "+lastName)
    
    var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
    pm.environment.set("totalPrice", totalPrice)
    console.log(totalPrice)
    
    var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
    pm.environment.set("depositPaid", depositPaid)
    console.log(depositPaid)
    
    //Date
    const moment = require('moment')
    const today = moment()
    pm.environment.set("checkin", today.add(1,'d').format("YYYY-MM-DD"))
    pm.environment.set("checkout",today.add(5,'d').format("YYYY-MM-DD") )
    
    var additionalNeeds = pm.variables.replaceIn("{{$randomNoun}}")
    pm.environment.set("additionalNeeds", additionalNeeds)
```
  **Request Body:** 
 ```console 
  {
	  "firstname" : "{{firstName}}",
	  "lastname" : "{{lastName}}",
	  "totalprice" : {{totalPrice}},
	  "depositpaid" : {{depositPaid}},
	  "bookingdates" : {
    	  "checkin" : "{{checkin}}",
    	  "checkout" : "{{checkout}}"
	  },
	  "additionalneeds" : "{{additionalNeeds}}"
  }
```
  **Response Body:**
 ```console 
  {
    "firstname": "Liana",
    "lastname": "Kuvalis",
    "totalprice": 426,
    "depositpaid": false,
    "bookingdates": {
        "checkin": "2024-12-27",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "alarm"
}
```

_**5. Delete Booking Record**_
 
### base_url:https://restful-booker.herokuapp.com
### Request URL:{{base_url}}/booking/{{ID}}
### Request Method: DELETE
### Response Body: None 

***Run Command:***
- Run Command for Console: 
```console 
newman run My_Collection_Day_1_Automate_Copy.postman_collection.json -e My_Collection_Day_1_Automate_Copy.postman_environment.json
```
- Run Command for Report: 
```console 
newman run My_Collection_Day_1_Automate_Copy.postman_collection.json -e My_Collection_Day_1_Automate_Copy.postman_environment.json -r cli,htmlextra
```

***Newman Report Summary:***
```console
file:///G:/SQA-2024/API%20TESTING/API_Project_Submission_1/Report/My_Collection_Day_1_Automate_Copy-2024-12-26-08-48-07-173-0.html
```
