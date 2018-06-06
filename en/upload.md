# Files upload and recognition
 <a name="parsing"></a>
 
`POST /account/{account_id}/upload` 
 
To upload a file send a request `multipart/form-data` with a file in parameter `file`.

To make sure that the file will be processed by the system of field recognition, one has to pass a header `X-File-Parse: true`. In this case the response will contain the fields `text`, `photo`, `fields`. 
 
 ```json
 {
     "id": 1329115,
     "name": "John Doe.pdf",

     "content_type": "application/pdf",
     "url": "https://store.huntflow.ru/uploads/t/f/9/tf96318hrjj5w9o5m9xuvwwwzbgbgave.pdf",
     "text": "Resume text",

     "photo": {
         "id": 1352599,
         "name": "image1.png",
         "url": "https://store.huntflow.ru/uploads/6/p/p/6ppzo93d8ic8slkzy7flrabqemnwtzpq.png",
         "content_type": "image/png"
     },
     "fields": {
         "position": "Front-end team lead",
         "email": "doe@gmail.com",
         "salary": 200000,
         "name": {
              "middle": "Michael",
              "last": "Doe",
              "first": "John"

         },
         "phones": [
             "+7 (926) 073-17-78"
         ],
         "birthdate": {
             "month": 7,
             "day": 20,
             "precision": "day",
             "year": 1984
         },
         "experience": [
             {
                 "position": "Mobile Team lead",
                 "company": "HeadHunter"
             },
             {
                 "position": "Senior frontend developer",
                 "company": "HeadHunter"
             }
         ]
     }
 }
 ```
 
 
Name | Type | Description
--- | --- | ---
id | number | Uploaded file ID
name | string | The name of the uploaded file
content_type | string | File type
url | string | Direct link to the file in Huntflow
text | string | Text file representation (if the resume is formated as a text)
photo | object | Object with a photo file if one was found in the recognized file, otherwise returns `null` 
fields | object | Object with recognized fields (fields description below) 

  
### Recognition system fields

 
Any of the fields stated below can be missing

  
Name | Type | Description
--- | --- | ---
name | object | Object with fields of first name, last name and middle name
position | string | Desired occupation
email | string | Email address
salary | number | Salary
phones | array | Array of phone numbers
birthdate | object | Object of the birth date. The field `precision` denotes the precision of the age recognition.
experience | array | Array with candidate's work experience.
