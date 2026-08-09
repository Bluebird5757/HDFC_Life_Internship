---
notion_id: 33f4fa76-9938-82ab-8586-019228fc17a6
notion_url: https://app.notion.com/p/Pydantic-Type-Validation-and-Data-Validation-33f4fa76993882ab8586019228fc17a6
title: Pydantic(Type Validation and Data Validation)
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-09T20:41:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-09T20:55:48.220Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

### What is it:- 


its an external library used for modelling the data  and is a data validation library in python


### Benefits:-


by modeling the data we get better IDE support for type hints and autocomplete, data validation, json serialization


### how to make a model of it:-


```python
from pydantic import Basemodel,EmailStr

class User(Basemodel):
    name:str
    email:str # use EmailStr here
    account_id:int

user_data=User(
    name="nikhil",
    email="abc@gmail.com", # if we had put another string that is not an email then it 
												   # would have passed but pydantic has another tool emailstr or
													 # field that ensures the data entered is an email
    account_id=1234 # if we had tried to create the account id using string then when we
								    # run the code there will be an error
)
# we can do this or we can directly unpack the data from an external dict
User(**user_data)# useful when getting the data from an external api

# if pydantic hasnt got your required field then we can create a custom validation by
# importing the validation tool and passing a function in the Model eg:-
# we want the account_id to be positive

from pydantic import BaseModel, validator,EmailStr

class User(Basemodel):
    name:str
    email:EmailStr
    account_id:int

    @validator('account_id')
    def validate_account_id(cls,value):
        if value<=0:
            raise ValueError(f"the account_id must be positive {value}")
        return value

user_data=User(
    name="nikhil",
    email="abc@gmail.com",
    account_id=-10 # this will raise an error
)

# the validator function is being called when we create the object
# BaseModel defines its own __init__ (Pydantic writes/generates this for you). When you 
# do:
user = User(name="nikhil", email="abc@gmail.com", account_id=1234)

# you're calling BaseModel.__init__, not just assigning attributes. Internally, 
# that __init__ does roughly this for every field:

# Look up the raw value passed in (1234 for account_id).
# Run it through the type coercion/parsing for that field's type (int).
# Check if any validator functions were registered for that field name.
# If yes, call each of them, passing cls and the value, in order.
# Take whatever comes back and store it as the actual attribute value.
# If any validator raises ValueError/TypeError/AssertionError, catch it and collect it 
# into a single ValidationError for the whole model.
```


we can convert the pydantic model into json and vice versa, we can also convert the json string into dict and vice versa 


## Pydantic vc Dataclasses 


Python already has dataclasses library with the help of which we can specify the data fields like this


```python
import dataclasses import dataclass

@dataclass # instead extending the BaseModel class we are using the dataclass decorator instead
class User(
    name:str
    email:str
    account_id:int
)
```


so what’s the difference


both give type hints but dataclass doesnt give data validation and the JSON serialization isnt very deep in dataclass whereas pydantic isnt built in


we could use type hinting in python when passing variables values to a function but that’s not very strong and will not raise errors even if wrong data type is passed


Three steps to use Pydantic in the code:-

- Define a Pydantic model/class that represents the ideal schema of the data
- Instantiate the model with raw input data (making object of the class)
- Pass the validated model object to functions or use it throughout your codebase

Pydantic can coerce the data as well meaning it can convert the data we are sending into the right data type


another example using optional, field,typing,Annotated


```python
from pydantic import BaseModel, Field
from typing import List, Dict,Optional,Annotated

class Patient(BaseModel):
    name:str=Field(max_length=30)
    age:int=Field(gt=0,lt=90) # can be used to set range as well
    weight:float=Field(gt=0)# used to set greater than 0
    #married:bool=False here we can set the default value 
    married:Optional[bool]=None # here optional is used if someone doesnt want to set 
															  # this field
    allergies:Optional[List[str]]=None # here List[str] is used not just list to validate 
																			 # the content in the list 
    contact_details:Dict[str,str]

patient_info={
        'name':'nitish',
        'age':30,
        'weight':72.5,
        'married':False,
        # 'allergies':['pollen','dust'],
        'contact_details':{
                'email':'nitish@gmail.com',
                'phone_no.':'999999999'
        }
}

def insert_patient_data(patient:Patient):
    print(patient.name)
    print(patient.age)
    print(patient.weight)
    print(patient.married)
    print(patient.allergies)
    print(patient.contact_details)

patient1=Patient(**patient_info)

insert_patient_data(patient1)
```


another use case for field function is adding metadata with the help of annotated


```python
name: Annotated[str,Field(max_length=30,title="Name of the patient",description="give the name of the patient in less than 30 words")]
```


when passing the variable value like weight we can pass the value as ‘72.5’ pydantic we automatically convert it but if you want to be more cautious then we can use strict in annotated like:-


```python
weight:Annotated[float,field(gt=0,strict=True)]# to avoid type coercion
```


in pydantic V2 field_validator for custom validation has come so when to use this eg usecase:-


    suppose we want to check the email being entered belongs to the employ of the HDFC Bank so for that we will use field validator:-


    ```python
    @field_validator('email')
    @classmethod
    def validate_email(cls,value):
     valid_domains=['hdfc.com','icici.com']
     domain_name=value.split('@')[-1]
     if domain_name not in valid_domains:
        raise ValueError("Not a valid domain")
     return Value
    ```


the field_validator has two modes after and before, by default its in after mode meaning the function is run after the type coercion is done on the variable but we can set its mode to before which uses the value of the variable before type coercion


instead of validation we can also alter the value of the variable before creating the object eg:-


```python
@field_validator('name')
@classmethod
def transform_name(cls,value):
	return value.upper()
```


what if we want to run data validation using multiple variables then we can’t use field_validator so we use model_validator eg:-


```python
@model_validator(mode='after')
def validate_emergency_contact(cls,model):
  if model.age>60 and 'emergency' not in model.contact_details:
     raise ValueError("No emergency details are there for age gt 60")
  return model
```


another concept Computed Fields eg:-
