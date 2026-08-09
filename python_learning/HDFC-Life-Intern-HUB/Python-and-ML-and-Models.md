---
notion_id: bed4fa76-9938-8271-9660-818e90184dcf
notion_url: https://app.notion.com/p/Python-and-ML-and-Models-bed4fa76993882719660818e90184dcf
title: Python and ML and Models
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-09T19:29:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-09T20:55:47.582Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

# Python


## List


Same as an array, but it can contain any data type, e.g. `["karan", 85, "delhi"]`.


arr[:4] same as [0:4]


arr[1:4] will return values on index 1,2,3


there are negative index also from back bnut starting from -1


[-3:-1] will return index -3,-2


list.index(idx,el) insert an element at an index


list.reverse() reverses in place and mutates the original list


list.remove(1) —> removes the first occurrence of an element


list.pop(idx) —> remove an element from the given index


## Strings are immutable


In Python, strings are immutable. 


## TUPLE


immutable


tuple=(2,1,3,4)


single value tuple=(2,) if we dont put a comma then the class will be int not tuple


tuple slicing is possible like lists


methods:-


tup.index(1) return the first occurence index of that number


# OOPS

- SELF - we use it to give reference to the object and it can be any name not just self but in python its best practice to write self. Python automatically passes it when an instance method is called.

```javascript
when passing say 

s1=student("NIKHIL")

and then calling 

s1.introduce()

python internally calls 

Student.introduce(s1) where s1 is the self 

	like this each object has its own copy of instance variables
```


Every object has its own state in python 


<u>Constructor are used to inject dependencies (something your class needs in order to its job) and  injection is giving what the class needs from outside like injecting antibodies from outside when oue body isnt producing one</u>


Duck typing lets code work. Inheritance helps developers organize and enforce a common design.


What's the difference between polymorphism and duck typing?


— **Polymorphism** means the same interface (`detect()`, `speak()`) can have different implementations.

— **Duck typing** is Python's way of achieving polymorphism. Python doesn't require a common parent class—it only checks whether the object has the required methods at runtime.


Inheritance can be used to share the code but also used to enforce the rules which is called abstract classes. Why use them so that every child class should have the same interface like same methods and all

- Instance Methods - does this method need object data then yes this is instance method
- Static Methods - Utility/helper Functions eg:- we need to calculate cosine similarity we don’t need object or class state and only paramaters

```python
class FaceUtils:
	
	@staticmethod
	def cosine_similarity(e1,e2):
		....
		
we dont need to mention self or cls in static method , its just logically related to the class
```

- Class Methods(Alternative Constructors)- if the method isnt using object data and either its creating an object or modifying class variables then its class methods

```python
suppose the company stores detector configuration in a json file
JSON
{
	"model":
	"threshold":
}

config = load_json()

detector = RetinaFaceDetector(
    config["model"],
    config["threshold"]
)


rather than this

class RetinafaceDetector:
	@classmethod
	def from_config(cls,config):
		return cls(
			config["model"],
			config["threshold"]
		)
		
	detector= RetinafaceDetector.from_config(config)
	
class methods modify class level information
```


# ML


### Confusion Matrix

- Precision - of all the instances of positive how many were positive and more precision means less FP mean that if a model if predicting a class positive it is more likely to be positive
- Recall - of all the actual positive cases how many did the model predict positive and more recall means less FN means model is predicting correctly more of positives

where we need high precision - where we cant afford to have more false positives - eg.:- face detection, spam detection like make a legitimate email spam is more dangerous than letting a spam email into inbox


where we need less false negative - medical diagnosis - a person having cancer is to be detected if the tumor is missed that is much more dangerous , than having more FP where a person who didnt have it go for a re-checkup


# Models


## Detection

- Reitnaface- 99.2 accuracy when ran on 1000 live images of the dataset and 99.6 precision and 99.7 recall
- Mediapipe - 98.8 accuracy and 1 precision and some recall

Embedding generation - apply filters on pixels (RGB channels) then convert into smaller matrix with channels as feature vectors and finally 512 feature vectors
