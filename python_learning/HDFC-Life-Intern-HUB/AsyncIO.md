---
notion_id: cf44fa76-9938-82ce-895d-81d69cdf7c27
notion_url: https://app.notion.com/p/AsyncIO-cf44fa76993882ce895d81d69cdf7c27
title: AsyncIO
source_file: /home/runner/work/HDFC_Life_Internship/HDFC_Life_Internship/python_learning/.notion.txt
source_line: 1
last_edited_time: '2026-08-09T19:30:00.000Z'
notion_parent:
  type: page_id
  page_id: 3b74fa76-9938-8028-a9a9-db4ca8197e34
fetched_at: '2026-08-15T14:43:45.073Z'
source_ref: https://app.notion.com/p/HDFC-Life-Intern-HUB-3b74fa7699388028a9a9db4ca8197e34?source=copy_link
---

# Concurrency and Parallelism:-


Concurrency is having one CPU core which context switches between programs which seems like tasks are progressing simultaneously . This is useful for I/O operations or webservers


Parallelism is having multiple CPU cores perform one task useful in rendering graphics and data analysis (Machine Learning)


Concurrency can be used to enable Parallelism as well


# Process and Threads


Programming is a sequence of instructions written in a programming language and process is simply an instance of a program that is being executed, each process has a separate memory space that helps in one process not corrupting another process but the execution time is increased when we need something from another process from different memory space


Thread is a unit of execution within  a process :- single threaded processes are there and multi threaded as well which have its own stack and registers with the help of which the process execute several things virtually at the same time, due to this the threads are in the same space which makes the execution fast but this also means that a single thread can corrupt the entire process


Chrome using tabs is an example of processes where each tab is a process so a tab getting corrupted doesnt effect other tabs and Apache is an example of threads which is used to handle clients


# Threading in Python


```python
import threading
import time
ls=[]
def count1(n):
    for i in  range(1,n+1):
        ls.append(i)
        time.sleep(0.05)

def count2(n):
    for i in  range(1,n+1):
        ls.append(i)
        time.sleep(0.05)

x=threading.Thread(target=count1,args=(5,)) # we wrote (5,) because a single tuple is
																						# written like this and without this its an 
																						# integer			
y=threading.Thread(target=count2,args=(5,))
x.start()
y.start()
x.join()
y.join()
print(ls)

# here in this program there are two threads x and y which we have intialized with 
# threading.Thread and given the target to start the thread we do .start() and .join() 
# to tell the program to run this thread before running print(ls), the result for the 
# print(ls)=[1,1,2,2,3,3,4,4,5,5]
# if we had written
x.start()
x.join()
y.start()
y.join()

# the result of this would have been [1,2,3,4,5,1,2,3,4,5] because first x ran and then
# y ran 

# the result can also be changed by changing the value of time.sleep()
```


> 💡 Remember when we run the .start() there is a an internal line called [self.ru](http://self.ru/)n() which actually runs the program so we can override that when we intialize the thread using init method we can create our own run method   
> def run   
>   
> and in this we can print and run some other function and do stuff like that ,  
>   
> the code:-


```python
import threading
import time

class myThread(threading.Thread):
    def __init__(self,threadID,name,count,delay):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name    
        self.count = count
        self.delay = delay

    def run(self):
        print("starting "+self.name+"\n")
        print_time(self.name,self.delay,self.count)
        print("ending "+self.name+"\n")

def print_time(name,delay,count):
    while count:
        time.sleep(delay)
        print("%s:%s %s" % (name,time.ctime(time.time()),count) + "\n")
        count -= 1

thread1=myThread(1,"Thread-1",10,1)
thread2=myThread(2,"Thread-2",5,2)

thread1.start()
thread2.start()
thread1.join()
thread2.join()
print("Exiting Main Thread")
```


locking example important:-


```python
import threading
import time
threadLock = threading.Lock()

class myThread(threading.Thread):
    def __init__(self,threadID,name,count,delay):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name    
        self.count = count
        self.delay = delay

    def run(self):
        print("starting "+self.name+"\n")
        threadLock.acquire() # <--- here the acquire method is used to acquire the lock if
											       # its free and start the payment thread
        print_time(self.name,self.delay,self.count)
        threadLock.release()
        print("ending "+self.name+"\n")

class myThread2(threading.Thread):
    def __init__(self,threadID,name,count,delay):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name    
        self.count = count
        self.delay = delay

    def run(self):
        threadLock.acquire() # due to this acquire method the thread cannot exceute the
													   # below line of code because here it acts as a stopper 
													   # and the lock is taken by the payment method and when the
													   # payment thread releases it then the sending email and loading
													   # page start simultaneously because we are taking the lock and
													   # immediately releasing, in this case anyone can get the lock first
													   # either the sending email or the loading email thread 
													   # the results could vary if we start the thread2 or thread3 first
				threadLock.release()
        print("starting "+self.name+"\n")
        print_time(self.name,self.delay,self.count)
        print("ending "+self.name+"\n")

def print_time(name,delay,count):
    while count:
        time.sleep(delay)
        print("%s:%s %s" % (name,time.ctime(time.time()),count) + "\n")
        count -= 1

thread1=myThread(1,"Payment",5,1)
thread2=myThread2(2,"Sending Email",10,1)
thread3=myThread2(3,"Loading Page",3,1)

thread1.start()
thread2.start()
thread3.start()
thread1.join()
thread2.join()
thread3.join()
print("Exiting Main Thread")
```


### Event loop :- a loop which manages tasks , if one task starts running and has to wait for some network response then another task can simultaneously and when the first task has got its input or output the task in returned immediately and starts executing


## AsyncIO


```python
import asyncio

#coroutine function
async def main(): #<-- when we write async that means its an asynchronous function and
									# its a coroutine
    print("run main coroutine")

main() # <-- if we run this then it returns a coroutine object or generates it and we 
						# will get an error if we call it like that because first we have to await 
						# it and does not execute it

# run the main coroutine
asyncio.run(main())
```


```python
import asyncio

# define a coroutine that simulates a time-consuming task
async def fetch_data(delay):
    print("Fetching data:")
    await asyncio.sleep(delay) # simualte an input output operation 
    print("Data fetched")
    return {"data":"some data"} # return some data

# define another coroutine that calls the first coroutine
async def main():
    print("start of the main coroutine")
    task=fetch_data(2)
    result = await task # <-- very important to understand the fact that the coroutine
										    # isnt executed untill we await it
    print(f"Received result:{result}")
    print("end of the main coroutine")

asyncio.run(main())

# the result for this code is 
start of the main coroutine
Fetching data:
Data fetched
Received result:{'data': 'some data'}
end of the main coroutine

# if the main part of the code would have been

async def main():
    print("start of the main coroutine")
    task=fetch_data(2)
    print("end of the main coroutine")
    result = await task
    print(f"Received result:{result}")

asyncio.run(main())

# then the result will be
start of the main coroutine
end of the main coroutine
Fetching data:
Data fetched
Received result:{'data': 'some data'}

# as you can see until we await the coroutine we dont get the output for the task and 
# the print statement runs first
```


but in the above code we have to wait for the task to finish if there had been two tasks like in this example:-


```python
import asyncio

async def fetch_data(delay,id):
    print("Fetching data....id:",id)
    await asyncio.sleep(delay)
    print("Data fetched id:",id)
    return {"data":"some data","id":id}

async def main():
    print("start of the main coroutine")
    task1=fetch_data(4,1)
    task2=fetch_data(1,2)
    result2=await task2
    result1= await task1
    print(f"Received result:{result1}")
    print(f"Received result:{result2}")
    print("end of the main coroutine")

asyncio.run(main())
```


we would have to wait for task 1 to complete to run task 2 so still not resource utilization


so for this we have got <u>_**Tasks**_</u>  which is a way to schedule to run a coroutine as soon as possible and to allow to run multiple coroutines simultaneously eg:-


```python
import asyncio

async def fetch_data(id,sleep_time):
    print(f"Coroutine {id} starting to fetch data.")
    await asyncio.sleep(sleep_time)
    return {"id":id,"data":f"Data from coroutine {id}"}

async def main():
    task1=asyncio.create_task(fetch_data(1,2))
    task2=asyncio.create_task(fetch_data(2,3))
    task3=asyncio.create_task(fetch_data(3,1))

    result1=await task1
    result2=await task2
    result3=await task3

    print(f"Received result 1: {result1}")
    print(f"Received result 2: {result2}")
    print(f"Received result 3: {result3}")

asyncio.run(main())

# now this code returns the output of all the three tasks in 3 seconds which otherwise 
# without create_task would have taken 6 seconds
```


but we have got a gather function that allows to run tasks concurrently rather than manually creating them using .create_task eg:-


```python
import asyncio

async def fetch_data(id,sleep_time):
    print(f"Coroutine {id} starting to fetch data.")
    await asyncio.sleep(sleep_time)
    return {"id":id,"data":f"Data from coroutine {id}"}

async def main():
    results= await asyncio.gather(
        fetch_data(1, 2),
        fetch_data(2, 1),
        fetch_data(3, 3)
    )
    for result in results:
        print(result)

asyncio.run(main())
```


but gather isnt very reliable in error handling because even if one coroutine has an error it will simply keep on running others which may get some undesired results so we use .TaskGroup() which  has in built error handling that will stop all other tasks if one fails making the application much more robust eg:-


```python
import asyncio

async def fetch_data(id,sleep_time):
    print(f"Coroutine {id} starting to fetch data.")
    await asyncio.sleep(sleep_time)
    return {"id":id,"data":f"Data from coroutine {id}"}

async def main():
    tasks = []
    async with asyncio.TaskGroup() as tg: # <-- async with is a context manager which give 
																					# access to tg		     
        for i,sleep_time in enumerate([2,1,3],start=1):
            task=tg.create_task(fetch_data(i,sleep_time))
            tasks.append(task)

    results=[task.result() for task in tasks]

    for result in results:
        print(f"Received: {result}")

asyncio.run(main())
```


another concept futures which is basically a promise of future result eg:-


```python
import asyncio

async def set_future_result(future, value):
    await asyncio.sleep(1)  # Simulate some asynchronous operation
    future.set_result(value)  # Set the result of the future
    print(f"Future result set to: {value}")

async def main():
    loop = asyncio.get_running_loop()
    future = loop.create_future()  # Create a Future object

    # Schedule the set_future_result coroutine to run
    asyncio.create_task(set_future_result(future, "Hello, World!"))

    # Wait for the future to be completed and get its result
    result = await future
    print(f"Received future result: {result}")
```


another concept Synchronization Primitives which allow us to Synchronize the execution of variosu coroutines for this we have synchro tools called asyncio.Lock() 


```python
import asyncio
shared_resource = 0
lock=asyncio.Lock()

async def modify_shared_resource():
    global shared_resource
    async with lock: # lock is simply locking this resource that is to be accessed one at a time and synchronizing coroutines to access this resource one at a time
        print(f"Resource before modification: {shared_resource}")
        shared_resource += 1
        asyncio.sleep(1)  # another task could be running when sleep occurs it cant start executing this critical part of the code until all of this finished and the lock is released
        print(f"Resource after modification: {shared_resource}")
        # critical section ends here

async def main():
        await asyncio.gather(*(modify_shared_resource() for _ in range(5)))

asyncio.run(main())
```


another concept is Semaphores which are used to allow multiple co routine to have access to the same object at the same time and we can decide how many we want that to be eg:-


```python
import asyncio

async def access_resource(semaphore,id):
    async with semaphore:
        print(f"Accessing resource {id}")
        await asyncio.sleep(1)
        print(f"releasing resource {id}")

async def main():
    semaphores=asyncio.semaphore(2)
    await asyncio.gather(*(access_resource(semaphores,i) for i in range(5)))

asyncio.run(main())
```


another function is event
