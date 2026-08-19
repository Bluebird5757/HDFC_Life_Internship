---
notion_id: d594fa76-9938-8355-a253-01c71f10f2c0
notion_url: https://app.notion.com/p/Decorators-d594fa7699388355a25301c71f10f2c0
title: Decorators
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-09T19:29:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-19T00:37:55.042Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

# First class function:


its said that a programming language is said to have first-class functions if it treats functions as first class citizens


first class citizen (programming):- aka first class objects, in a programming language is an entity which supports all the operations generally available to other entities. these operations typically include being passed as an argument, returned from a function, and assigned to a variable


```python
#like
def square(x):
	return x*x
	
f=square(5)
print(f)
print(square)

# will give the same output as 

def square(x):
	return x*x
	
f=square #this is one of the aspects to what it means to be first class functions
print(f(5))
print(square)

#as f becomes the function but we cant do f=square() that means we are going to 
#execute the function
```


```python
#another example

def square(x):
    return x*x

def cube(x):
    return x*x*x

def my_map(find,arr_list):
    result=[]
    for i in arr_list:
        result.append(find(i))
    return result
    
squares=my_map(cube,[1,2,3,4,5]) # as you can see we pass the cube as it is no need for 
#paranthesis and similarly we could have written square instead of cube
print(squares)
```


```python
def logger(msg):
    
    def log_message():
        print("log:",msg)
        
    return log_message
    
log_hi=logger("hi")
log_hi()

#calling another function like this inside another function be useful 
#we can another example to understand

def html_tag(tag):

    def wrap_text(msg):
        print('{0}<{1}>{0}'.format(tag,msg))
        
    return wrap_text  # wrap_text this function we can pass it around anywhere
									    # we can assign them to any variables, pass them as arguments 
									    # like in square exmple above, we can return these functions
									    # from another functions
    
print_h1=html_tag('h1')
print_h1('test headline')
print_h1('another headline')

print_p=html_tag('p')
print_p('test paragraph')
```


# Closures


A closure is a record storing a function together with an env: a mapping associating each free variable of the function with the value or storage location to which the name was bound when the closure was created. A closure, unlike a plain function, allows the function to access those captured variables through the closure’s reference to them, even when the function is invoked outside their scope


```python
def outer_fun():
	message='Hi'
	
	def inner_fun():
		print(message)
		
	return inner_func() # here we are returning with paranthesis but in the above example 
											# we were returning without it because in the above example when 
											# the logger function was returning the log_message then it was 
											# getting stored in the log_hi variable basically the log_hi 
											# variable had become the log_message function so we can call it 
											# from outside the function here also  
	
outer_func()

																				 # if we do
											my_func=outer_func # then we dont need the paranthesis for the inner_func
```


so in simple terms closure is an inner_func that has access to the variables in its local scope in which it was created even after the outer function has finished executing.


we can use them for logging


## Decorators:-


instead of a message being passed in what if we pass a function in the decorator function and that’s what a decorator function is


```python
def decorator_function(original_function):
	def wrapper_function():
		return original_function()
	return wrapper_function()

def display():
    print("lol")
    
decorator_function(display)

	# this will display the result of the display function
	
	# we can also print a message inside the wrapper function before and 
	# after the orginal function
	
	def decorator_function(original_function):
    def wrapper_function():
        print("wrapper executed this before {}".format(original_function.__name__))
        return original_function()
    return wrapper_function()

def display():
    print("lol")


decorator_function(display)

# this is one of the syntaxes and another one can be:-
# this is one of the synataxes that is used common 

def decorator_function(original_function):
    def wrapper_function():
        print("wrapper executed this before {}".format(original_function.__name__))
        return original_function()
    return wrapper_function()

@decorator_function # here the decorator_function is just the name of the function we
										# we have used above, the above function name can be anything we have 
										# we have used decorator name just for explanation
def display():
    print("lol")
    
# using @decorator_function is same as using display=decorator_function(display)

# we can also pass arguments in the decorator function inside the wrapper function like:-

def decorator_function(original_function):
    def wrapper_function(*args,**kwargs):
        print("wrapper executed this before {}".format(original_function.__name__))
        return original_function(*args,**kwargs)
    return wrapper_function

@decorator_function
def display():
    print("display function ran")

@decorator_function
def display_info(name,age):
    print("display info ran with arguments ({},{})".format(name,age))

display()
display_info('khan',30)

# where *args,**kwargs are used for any number of arguments and key word arguments 
# we can use different naming convention for args and kwargs but this is the standard
```


another example for syntax is using classes as decorators but this is less common


```python
def decorator_function(original_function):
    def wrapper_function(*args,**kwargs):
        print("wrapper executed this before {}".format(original_function.__name__))
        return original_function(*args,**kwargs)
    return wrapper_function

class decorator_class(object):
    def  __init__(self, original_function):
        self.original_function=original_function

    def __call__(self,*args,**kwargs):
        print("call method executed this before {}".format(self.original_function.__name__))
        return self.original_function(*args,**kwargs)

@decorator_class
def display():
    print("display function ran")

@decorator_class
def display_info(name,age):
    print("display info ran with arguments ({},{})".format(name,age))

display()
display_info('khan',30)

# here orginal_function is intialized using the self and the call method is doing the 
# same thing as the wrapper function
```


another example  ( practical examples ) : -


```python
def my_logger(orig_func):
    import logging
    logging.basicConfig(filename='{}.log'.format(orig_func.__name__),level=logging.INFO)

    def wrapper(*args,**kwargs):
        logging.info(
            'Ran with args: {}, and kwargs: {}'.format(args,kwargs)
        )
        return orig_func(*args,**kwargs)

    return wrapper

def my_timer(orig_func):
    import time

    def wrapper(*args,**kwargs):
        t1=time.time()
        result=orig_func(*args,**kwargs)
        t2=time.time()-t1
        print("{} ran in {}".format(orig_func.__name__,t2))
        return result

    return wrapper

@my_logger
def display_info(name,age):
    print("display_info with args ({},{})".format(name,age))

display_info("khan",30)

# when i run this code it will create a log file with the orig_func name and log in the
# info being passed and also give the output of the orig_func

# ** if i had wanted to call the original function first in my_logger i would have to write
# the code same as timer , i cant do return orig_func that will exit the function and the
# logging wont happen

import logging
    logging.basicConfig(filename='{}.log'.format(orig_func.__name__),level=logging.INFO)

    def wrapper(*args,**kwargs):
		    result=orig_func(*args,**kwargs)
        logging.info(
            'Ran with args: {}, and kwargs: {}'.format(args,kwargs)
        )
        return result

    return wrapper

# if we stack the decorator like
# @my_logger
# @my_timer 
# unexpected reults will come because by stacking the decorators we are simply chaining
# the decorators like my_logger(my_timer(display_info)) here in this my_timer gets
# converted into wrapper(*args,**kwargs) or the display.__name__ becomes wrapper it 
# takes wrapper so the orig_func no longer remains display_info and vice_versa
# to solve this issue we can use the wraps function from the functools module like this:-

def my_logger(orig_func):
    import logging
    logging.basicConfig(filename='{}.log'.format(orig_func.__name__),level=logging.INFO)
		
		@wraps(orig_func)  # <-- highlight
    def wrapper(*args,**kwargs):
        logging.info(
            'Ran with args: {}, and kwargs: {}'.format(args,kwargs)
        )
        return orig_func(*args,**kwargs)

    return wrapper

def my_timer(orig_func):
    import time
		
		@wraps(orig_func)  # <-- highlight
    def wrapper(*args,**kwargs):
        t1=time.time()
        result=orig_func(*args,**kwargs)
        t2=time.time()-t1
        print("{} ran in {}".format(orig_func.__name__,t2))
        return result

    return wrapper

@my_logger
def display_info(name,age):
    print("display_info with args ({},{})".format(name,age))

display_info("khan",30)
```


## Decorators with arguments


```python
def prefix_decorator(prefix)
	def decorator_function(original_function):
	    def wrapper_function(*args,**kwargs):
			    print(prefix)
	        print("wrapper executed this before {}".format(original_function.__name__))
	        return original_function(*args,**kwargs)
	    return wrapper_function
	  return decorator_function

@prefix_decorator("TESTING:") # <---- arguments passed inside decorator
def display_info(name,age):
    print("display info ran with arguments ({},{})".format(name,age))

display_info('khan',30)
```
