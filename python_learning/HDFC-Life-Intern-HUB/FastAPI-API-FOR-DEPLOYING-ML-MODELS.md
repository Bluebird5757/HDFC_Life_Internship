---
notion_id: 3b84fa76-9938-804e-8f6d-c7b1094e603e
notion_url: https://app.notion.com/p/FastAPI-API-FOR-DEPLOYING-ML-MODELS-3b84fa769938804e8f6dc7b1094e603e
title: FastAPI(API FOR DEPLOYING ML MODELS)
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-14T14:07:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-17T00:38:42.068Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

### What is an api:-


kind of like a connector that connects two software eg:- frontend and backend (software dev usecase) to communicate with each other using some rules and protocols and data formats


### why do api exist:-


to solve the problem of monolithic architecture which has the property pf tight coupling, because of which we cant give our data outside our application eg :- irctc has got the data of the trains now ixigo and makemytrip type company want that data to show on their app so they tell irctc we will give money for that but irctc being in monolithic architecture cant give the access to backend or database


### how does this problem is getting solved:-


first we stop using the monolithic architecture and start using different softwares for the backend and the frontend, now we have got a layer of API in front of the backend


API is basically a set of endpoints which are publicly exposed on the internet and anyone can access them and we can access the function in the backend with the help of the APIs and we can apply constraints in the API 


as the API is a connector it has to follow some protocols which is the http and the data format in which the api replies is the json which is a universal format any language can understand it, access it and use it 


another problem which the api is solving is that earlier companies had to make different backends for the ios,android,web app but now the backend and in between only the api is required and there are different frontends 


### ML Domain API


in software the most important is the database but in ML domain the ML model is the most important 


## FastAPI


It is a modern, high-performance web framework for building APIs with Python


its built on Starlette and pydantic 


Starlette manages how your API receives request and sends back responses


Pydantic is used for validation


Fast to run API:-

- so there is an interpreter that is between the web server and the api code which changes the http request into a python understandable code :- in case of Flask its wsgi(web server gateway interface) which has a problem that is synchronous and is blocking nature meaning it can process one request at a time whereas FastAPI uses ASGI which is asynchronous. WSGI uses werkzeug as the library and ASGI uses Starlette as the library
- the webserver which is used in Flask is Gunicorn and the FastAPI has uvicorn

Fast to code:-

- Automatic Input Validation
- Auto - Generated Interactive Documentation
- Seamless Integration with Modern Ecosystem(ML/DL, OAuth, JWT)

### Installation


first create a virtual env and run it 


inside that run pip install pydantic fastapi uvicorn


the sample code:-


```python
from fastapi import FastAPI

app=FastAPI()# object creation of the fastapi

@app.get('/') # this is the home endpoint
def hello():
    return {'message':'Hello world'}

@app.get('/about') # this is the exposed endpoints 
def about():
    return {'message':'This is a FastAPI application.'}
```


to run this code we need to run in terminal


uvicorn main:app —reload 


app here is the fastapi object we have created and reload is used so that every time we edit the code it keeps on saving automatically and we dont have to run the code again


### HTTPS Methods


the software can be static as well as dynamic and with any software we can perform only 4 types of operations CRUD(create, retrieve , update, delete)


websites are also software but they are installed on a machine(server) and used on another machine(client) and this conversation is done through protocols which is the http


### REST API


this is just the rules on how to design an api the get, post, put, delete are all endpoints which follow the REST principles regardless of the programming language or framework used, here FastAPI is a framework like express which are used for backend and react is a framework used for frontend.


its not important that the backend framework used is REST its just an architecture that is used mostly for general api 


Node.js is a runtime that lets you run JavaScript outside the browser.


**Express is specifically a JavaScript framework**, so you cannot use Python with Express.

 


| Language | Backend frameworks |
| -------- | ------------------ |


| **JavaScript** | Express, NestJS, Fastify |
| -------------- | ------------------------ |


| **Python** | FastAPI, Flask, Django |
| ---------- | ---------------------- |


| **Java** | Spring Boot |
| -------- | ----------- |


| **C#** | ASP.NET Core |
| ------ | ------------ |


| **Go** | Gin, Fiber |
| ------ | ---------- |


| **PHP** | Laravel |
| ------- | ------- |


### Path Params


they are dynamic segments of a URL path used to identify a specific resource


```python
from fastapi import FastAPI
import json

app=FastAPI()
def load_data():
    with open('patients.json','r') as f:
        data = json.load(f)
    return data

@app.get('/') 
def hello():
    return {'message':'Patient Management System API'}

@app.get('/about')
def about():
    return {'message':'A fully functional API to manage the patients and their medical records.'}

@app.get('/view')
def view():
    return load_data()

@app.get('/patients')
def view_patients():
    return {'error':'please provide patient id'}

@app.get('/patients/{patient_id}') # this here is a dynamic part of the url 
def view_patient(patient_id):
    data=load_data()
    if patient_id in data:
        return data[patient_id]
    return {'error':'patient id doesn\'t exist'}
```


we can also access the data by going to the host url/docs([http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)) where we will get the interface 


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/6adb3a0a-3be0-4d41-8c0b-4d05e575ebf7/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIA32CLX%2F20260817%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260817T003837Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJHMEUCIQCOc7pUtlwS3kCe8uIQXQQcz3pI30TfDowIw20%2BtZPbaAIgHZyHMm454noWMRmj0n4vH9WctWWS9cEJXeRrCmIRuY8q%2FwMIORAAGgw2Mzc0MjMxODM4MDUiDLFwgPpA0BIf5pRB9yrcA0aswngsjjqjma1vXt%2F%2BEu7l3ajGZ2ITzJYKygykNFibKtTd9iIO1WmbQqJ%2FkHavqjleJ%2BBZSuNwe6EToF2jW5ZUvlz3is8Pif1rX2XTdMIM73PWC%2BLspbcNucvIYHxTTTUiX4UM7wYUusgx6t1qq%2BsHEs2FFcKFcBG1Us9ku39ubnWk6X%2BC1Q0SAU7ExLqrcQfSdOBW%2Be7m0u%2B5iaUdTnjcSgOqItdOhzzBiMr%2BMmQA5RWIpddm1YksmI2J8veSt1%2FVmBByQXy3wTp7sHP1yT8My%2BB%2FF1dgoMMePA2G4%2FzseCeI8xahOZwuDReZx%2BZTmNrSZFpvcDp6iX3NdQeXJvu2bXZ1RGukb7g5bOreoXd%2FT32CPr%2F1CKQer3ELnxkYy%2FhJeCugbYSWM9Y9NvM7iMdCDjVVw8GF3TIPs9F5VHF2azloD2x%2BmH3NgMdOLUWTFDsgsTmLkV8%2BrViKCuLZ0FI1EnhHBjJnefZPjyyGlbzBWG8RQQBlXcgZmRV5lLO4u%2BsYXPzbqcyHeTRezqUYZ7uYqmlxYEojZq2lztQzTVYIeV9YeRxg2k7nitf7lN%2FYqXwR1YOHJ%2BazpmV9ZXkg77KWRA3GBqzXPVOLub7ntj7AbaEm%2BRBBNA%2BYwKKGMNaTidQGOqUBAaG%2F%2FhKp61tc4469FlPg6KUnMQFxE73Bdw9%2Bpc8hGqXcr6yr99gsMwy%2Fge%2F%2BHFIwz%2Fy%2FkRa9w5sAUyptGe9GV%2FHRE8t%2B4SEK0p0UE05hEI91KLMYSHlFuR%2B8jov1QcCDAdiRu46wtb6tI7IXhzm5ikTBPEWUusovWMOy5WC15Xuui1NvCM40SG1iF6tmfLZDa5P8xc7l5%2B2FHqI8sJ%2FGII3TVhq7&X-Amz-Signature=70bb497ed6fc37a2d0a2c20079d405f145179b9efa7667d908be8deb185a2123&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


after clicking on the patients/patient_id we will get a placeholder where we can write the patient_id and that will fetch us the patient details


Path function in a fastapi (we have to import it from fastapi)


it is used to provide metadata, validation rules, and documentation hints for path parameters in your API endpoints 


Title


Description


Example


ge,gt,le,lt


Min_length


Max_length


regex


```python
@app.get('/patients/{patient_id}')
def view_patient(patient_id:str=Path(...,description='The patient id to view the details',example='P001')):
    data=load_data()
    if patient_id in data:
        return data[patient_id]
    return {'error':'patient id doesn\'t exist'}
```


in the end we are enhancing the readability of the client in the docs like:-


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/b43b79ee-761e-4a14-846d-b4df4b86bba0/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIA32CLX%2F20260817%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260817T003837Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJHMEUCIQCOc7pUtlwS3kCe8uIQXQQcz3pI30TfDowIw20%2BtZPbaAIgHZyHMm454noWMRmj0n4vH9WctWWS9cEJXeRrCmIRuY8q%2FwMIORAAGgw2Mzc0MjMxODM4MDUiDLFwgPpA0BIf5pRB9yrcA0aswngsjjqjma1vXt%2F%2BEu7l3ajGZ2ITzJYKygykNFibKtTd9iIO1WmbQqJ%2FkHavqjleJ%2BBZSuNwe6EToF2jW5ZUvlz3is8Pif1rX2XTdMIM73PWC%2BLspbcNucvIYHxTTTUiX4UM7wYUusgx6t1qq%2BsHEs2FFcKFcBG1Us9ku39ubnWk6X%2BC1Q0SAU7ExLqrcQfSdOBW%2Be7m0u%2B5iaUdTnjcSgOqItdOhzzBiMr%2BMmQA5RWIpddm1YksmI2J8veSt1%2FVmBByQXy3wTp7sHP1yT8My%2BB%2FF1dgoMMePA2G4%2FzseCeI8xahOZwuDReZx%2BZTmNrSZFpvcDp6iX3NdQeXJvu2bXZ1RGukb7g5bOreoXd%2FT32CPr%2F1CKQer3ELnxkYy%2FhJeCugbYSWM9Y9NvM7iMdCDjVVw8GF3TIPs9F5VHF2azloD2x%2BmH3NgMdOLUWTFDsgsTmLkV8%2BrViKCuLZ0FI1EnhHBjJnefZPjyyGlbzBWG8RQQBlXcgZmRV5lLO4u%2BsYXPzbqcyHeTRezqUYZ7uYqmlxYEojZq2lztQzTVYIeV9YeRxg2k7nitf7lN%2FYqXwR1YOHJ%2BazpmV9ZXkg77KWRA3GBqzXPVOLub7ntj7AbaEm%2BRBBNA%2BYwKKGMNaTidQGOqUBAaG%2F%2FhKp61tc4469FlPg6KUnMQFxE73Bdw9%2Bpc8hGqXcr6yr99gsMwy%2Fge%2F%2BHFIwz%2Fy%2FkRa9w5sAUyptGe9GV%2FHRE8t%2B4SEK0p0UE05hEI91KLMYSHlFuR%2B8jov1QcCDAdiRu46wtb6tI7IXhzm5ikTBPEWUusovWMOy5WC15Xuui1NvCM40SG1iF6tmfLZDa5P8xc7l5%2B2FHqI8sJ%2FGII3TVhq7&X-Amz-Signature=351c74377193f44bdabec5fca15ba71c5da9ec4cc49c2d534e8b42c8b65c6ce6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


### HTTP Status Code:-


they are 3 digit numbers returned by a web server (like FastAPI ) to indicate the result of a client’s request (like from a browser or API consumer). mainly there are 4 types of it:-


2xx                      Success         the request was successfully received and processed


3xx                      redirection    further action needs to be taken


4xx                      client error   something is wrong with the request from the client


5xx                      server error  something went wrong on the server side 


200  ok                       get or post 


201 created               post


204 no comments  delete


400 bad request


401 unauthorized


403 Forbidden


404 not found


500 Internal server error    generic failure   something broke on the server


502 bad gateway                 gateway(like nginx) failed to reach backend


503 service unavailable      server is overloaded


so in the project there is a problem when the client asks for a patient_id which is not there in the database the status code being shown is still 200 


![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90e4fa76-9938-8198-89c0-0003c2ea03dc/658973b8-d60b-4140-a359-610f26756011/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIA32CLX%2F20260817%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260817T003837Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJHMEUCIQCOc7pUtlwS3kCe8uIQXQQcz3pI30TfDowIw20%2BtZPbaAIgHZyHMm454noWMRmj0n4vH9WctWWS9cEJXeRrCmIRuY8q%2FwMIORAAGgw2Mzc0MjMxODM4MDUiDLFwgPpA0BIf5pRB9yrcA0aswngsjjqjma1vXt%2F%2BEu7l3ajGZ2ITzJYKygykNFibKtTd9iIO1WmbQqJ%2FkHavqjleJ%2BBZSuNwe6EToF2jW5ZUvlz3is8Pif1rX2XTdMIM73PWC%2BLspbcNucvIYHxTTTUiX4UM7wYUusgx6t1qq%2BsHEs2FFcKFcBG1Us9ku39ubnWk6X%2BC1Q0SAU7ExLqrcQfSdOBW%2Be7m0u%2B5iaUdTnjcSgOqItdOhzzBiMr%2BMmQA5RWIpddm1YksmI2J8veSt1%2FVmBByQXy3wTp7sHP1yT8My%2BB%2FF1dgoMMePA2G4%2FzseCeI8xahOZwuDReZx%2BZTmNrSZFpvcDp6iX3NdQeXJvu2bXZ1RGukb7g5bOreoXd%2FT32CPr%2F1CKQer3ELnxkYy%2FhJeCugbYSWM9Y9NvM7iMdCDjVVw8GF3TIPs9F5VHF2azloD2x%2BmH3NgMdOLUWTFDsgsTmLkV8%2BrViKCuLZ0FI1EnhHBjJnefZPjyyGlbzBWG8RQQBlXcgZmRV5lLO4u%2BsYXPzbqcyHeTRezqUYZ7uYqmlxYEojZq2lztQzTVYIeV9YeRxg2k7nitf7lN%2FYqXwR1YOHJ%2BazpmV9ZXkg77KWRA3GBqzXPVOLub7ntj7AbaEm%2BRBBNA%2BYwKKGMNaTidQGOqUBAaG%2F%2FhKp61tc4469FlPg6KUnMQFxE73Bdw9%2Bpc8hGqXcr6yr99gsMwy%2Fge%2F%2BHFIwz%2Fy%2FkRa9w5sAUyptGe9GV%2FHRE8t%2B4SEK0p0UE05hEI91KLMYSHlFuR%2B8jov1QcCDAdiRu46wtb6tI7IXhzm5ikTBPEWUusovWMOy5WC15Xuui1NvCM40SG1iF6tmfLZDa5P8xc7l5%2B2FHqI8sJ%2FGII3TVhq7&X-Amz-Signature=f67386ca5bd35e18ed02630560afba996af8cecd88fe0e168b5bbaec7aa3219c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


to solve this we have got a custom HTTPException class with the help of which we can raise graceful errors with our custom message


```python
@app.get('/patients/{patient_id}')
def view_patient(patient_id:str=Path(...,description='The patient id to view the details',example='P001')):
    data=load_data()
    if patient_id in data:
        return data[patient_id]
    raise HTTPException(status_code=404,detail=f'Patient with id {patient_id} not found')
```


### Query Parameters


they are optional key value pairs appended to the end of the url used to pass additional data to the server in an http request. they are typically employed for operations like filtering, sorting, searching, and pagination, without altering the endpoint path itself.


eg:-/patients?city=Delhi & sort_by=age 


there is also a Query() utility function provided by fastAPI to declare, validate, and document query parameters in your API endpoints.


It allows you to:

- set default values
- enforce validation values
- add metadeta like description, title, examples

### Request Body


it is the portion of an HTTP request that contains data sent by the client to the server, it is typically used in http methods such as POST, PUT to transmit structured data(eg. JSON,XML,form-data) for the purpose of creating or updating resources on the server . The server parses the request body to extract the necessary information and perform the intended operation.


three steps to create or update the data:-

- send the data on the server
- validation of the data through pydantic
- json file→ new record added

## POST


to create the body using pydantic model


```python
class Patient(BaseModel):
    id:Annotated[str,Field(...,description='The unique patient id',example='P001')]
    name:Annotated[str,Field(...,description='The name of the patient',example='John Doe')] 
    gender:Annotated[Literal['Male','Female','others'],Field(...,description='The gender of the patient')]
    city:Annotated[str,Field(...,description='The city of the patient',example='New York')]
    age:Annotated[int,Field(...,gt=0,description='The age of the patient')]
    height:Annotated[float,Field(...,gt=0,description='The height of the patient in meters')]
    weight:Annotated[float,Field(...,gt=0,description='The weight of the patient in kg')]

    @computed_field(alias='bmi') # alias is used so that when creating a dict the name
															   # as bmi and not compute_bmi
    @property
    # here the property treats the method as an attribute so we can use it in compute_verdict
    # as an attribute
    def compute_bmi(self)->float:
        return round(self.weight/(self.height)**2,2)

    @computed_field(alias='verdict')
    @property
    def compute_verdict(self)->str:
        if self.compute_bmi<18.5:
            return 'Underweight'
        elif self.compute_bmi<25:
            return 'Normal weight'
        elif self.compute_bmi<30:
            return 'Overweight'
        else:
            return 'Obese'


@app.post('/create')
def create_patient(patient:Patient):
    #load existing data
    data=load_data()
    #check if patient already exist
    if patient.id in data:
        raise HTTPException(status_code=400,detail='Patient already exists')
    #new patient add in the database
    data[patient.id]=patient.model_dump(exclude=['id'],by_alias=True)# pydantic object to dict
																										   # by_alias used for setting the alias 
																											 # passed in the methods in the model object
    # save into the json file
    save_data(data)
    return JSONResponse(status_code=201,content={'message':'Patient created successfully','patient':data[patient.id]})
```


## PUT


the steps for updating the account are:-

    - here we will be getting the patient_id through the path params (url ka part) and the request body so after that:-
        - new pydantic model for the patient
        - new data comes update the existing it in update

```python
@app.put('/update/{patient_id}')
def update_patient(patient_update:PatientUpdate,patient_id:str=Path(...,description='The patient id to update',example='P001')):
    data=load_data()
    if patient_id not in data:
        raise HTTPException(status_code=404,detail='the id is not there')
    existing_patient_data=data[patient_id]
    updated_patient_info=patient_update.model_dump(exclude_unset=True)
    for key,value in updated_patient_info.items():
        existing_patient_data[key]=value
        
    #existing_patient_data->pydantic object->updated bmi+verdict->dict
    
    existing_patient_data['id']=patient_id
    patient_pydantic_object=Patient(**existing_patient_data)
    existing_patient_data=patient_pydantic_object.model_dump(exclude='id') 
    data[patient_id]=existing_patient_data
    # save 
    save_data(data)
```


## Delete


```python
@app.delete('/delete/{patient_id}')
def patient_delete(patient_id:str=Path(...,description='delete the patient_record',example='P001')):
    data=load_data()
    if patient_id not in data:
        raise HTTPException(status_code=404,detail='id not there')
    del data[patient_id]
    save_data(data)
    return JSONResponse(status_code=200,content={'message':'Patient deleted successfully'})
```


## The whole code


```python
from fastapi import FastAPI,Path,HTTPException,Query
from pydantic import BaseModel,Field,computed_field
from fastapi.responses import JSONResponse
from typing import List,Annotated,Literal,Optional
import json

class Patient(BaseModel):
    id:Annotated[str,Field(...,description='The unique patient id',example='P001')]
    name:Annotated[str,Field(...,description='The name of the patient',example='John Doe')] 
    gender:Annotated[Literal['Male','Female','others'],Field(...,description='The gender of the patient')]
    city:Annotated[str,Field(...,description='The city of the patient',example='New York')]
    age:Annotated[int,Field(...,gt=0,description='The age of the patient')]
    height:Annotated[float,Field(...,gt=0,description='The height of the patient in meters')]
    weight:Annotated[float,Field(...,gt=0,description='The weight of the patient in kg')]

    @computed_field(alias='bmi')
    @property
    def compute_bmi(self)->float:
        return round(self.weight/(self.height)**2,2)

    @computed_field(alias='verdict')
    @property
    def compute_verdict(self)->str:
        if self.compute_bmi<18.5:
            return 'Underweight'
        elif self.compute_bmi<25:
            return 'Normal weight'
        elif self.compute_bmi<30:
            return 'Overweight'
        else:
            return 'Obese'

class PatientUpdate(BaseModel):
        name:Annotated[Optional[str],Field(default=None)] 
        gender:Annotated[Optional[Literal['Male','Female','others']],Field(default=None)]
        city:Annotated[Optional[str],Field(default=None)]
        age:Annotated[Optional[int],Field(default=None,gt=0)]
        weight:Annotated[Optional[float],Field(default=None,gt=0)]
        height:Annotated[Optional[float],Field(default=None,gt=0)]



app=FastAPI()
def load_data():
    with open('patients.json','r') as f:
        data = json.load(f)
    return data

def save_data(data):
    with open('patients.json','w') as f:
        json.dump(data,f,indent=4)

@app.get('/') 
def hello():
    return {'message':'Patient Management System API'}

@app.get('/about')
def about():
    return {'message':'A fully functional API to manage the patients and their medical records.'}

@app.get('/view')
def view():
    return load_data()

@app.get('/patients')
def view_patients():
    return {'error':'please provide patient id'}

@app.get('/patients/{patient_id}')
def view_patient(patient_id:str=Path(...,description='The patient id to view the details',example='P001')):
    data=load_data()
    if patient_id in data:
        return data[patient_id]
    raise HTTPException(status_code=404,detail=f'Patient with id {patient_id} not found')

@app.get('/sort')
def sort_patients(sort_by:str=Query(...,description='Sort on the basis of height ,weight or bmi'),order:str=Query('asc',description='sort in asc or desc order')):
    valid_fields=['height','weight','bmi']
    if sort_by not in valid_fields:
        raise HTTPException(status_code=400,detail=f'Invalid sort field. Valid fields are {valid_fields}')
    if order not in ['asc','desc']:
        raise HTTPException(status_code=400,detail='Invalid order. Valid orders are asc or desc')
    data=load_data()
    sort_order=True if order=='desc' else False
    sorted_data=sorted(data.values(),key=lambda x:x.get(sort_by,0),reverse=sort_order)
    return sorted_data

@app.post('/create')
def create_patient(patient:Patient):
    #load existing data
    data=load_data()
    #check if patient already exist
    if patient.id in data:
        raise HTTPException(status_code=400,detail='Patient already exists')
    #new patient add in the database
    data[patient.id]=patient.model_dump(exclude=['id'],by_alias=True)# pydantic object to dict
    # save into the json file
    save_data(data)
    return JSONResponse(status_code=200,content={'message':'Patient created successfully'})

@app.put('/update/{patient_id}')
def update_patient(patient_update:PatientUpdate,patient_id:str=Path(...,description='The patient id to update',example='P001')):
    data=load_data()
    if patient_id not in data:
        raise HTTPException(status_code=404,detail='the id is not there')
    existing_patient_data=data[patient_id]
    updated_patient_info=patient_update.model_dump(exclude_unset=True)
    for key,value in updated_patient_info.items():
        existing_patient_data[key]=value
        
    #existing_patient_data->pydantic object->updated bmi+verdict->dict
    
    existing_patient_data['id']=patient_id
    patient_pydantic_object=Patient(**existing_patient_data)
    existing_patient_data=patient_pydantic_object.model_dump(exclude='id') 
    data[patient_id]=existing_patient_data
    # save 
    save_data(data)
    return JSONResponse(status_code=200,content={'message':'Patient updated successfully'})

@app.delete('/delete/{patient_id}')
def patient_delete(patient_id:str=Path(...,description='delete the patient_record',example='P001')):
    data=load_data()
    if patient_id not in data:
        raise HTTPException(status_code=404,detail='id not there')
    del data[patient_id]
    save_data(data)
    return JSONResponse(status_code=200,content={'message':'Patient deleted successfully'})
```
