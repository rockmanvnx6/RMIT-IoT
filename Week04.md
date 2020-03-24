# Week 04: Database programming, cron jobs, and api

## Sense HAT tasks

Look at week 4 code.

>   You can toggle the Gama to low light mode, useful for dark environment

Sometimes, you need to play with `gamma`

>   -   Gamma correction is a nonlinear operation used to encode and decode luminance in still image systems



Sense hat python api uses 8 bit (0-255) colours for r,g,b.

When these are written to the linux frame buffer, they're a bit shifted into RBG 5 6 5



The driver is then converts them into RGB 5 5 5 



### Sensors

IMU(inertial measurement unit) sensor is a combination of 3 sensors, each with an x,y,z axis.

-   Gyroscope, accelerometer, magnetometer(compas)



### Calibiration

For the temperature sensehat, it might not be entirely correct because of the thermal conduction from PiCPU to the humidity and pressure sensors on sense hat.

**You will need to read the os temperature and calibrate it.**



## Python

### Mysql

-   More scalable comparing to Sqlite
-   can be tuned more easily
-   supports user management and permissions
-   can be used in client server architectures where a database client must access a database remotely.

### Parameterised SQL

The idea is we want to send a parameter instead of the whole sql

This will prevent SQL Injection attack.

```python
sql_insert_query = """ INSERT INTO Employee (id, Name, salary) VALUES (%s,%s,%s)"""

insert_tuple_1 = (1, "Tom", 9000)
insert_tuple_2 = (2, ”Alice", 5000)
cursor.execute(sql_insert_query, insert_tuple_1) # give a parameter
cursor.execute(sql_insert_query, insert_tuple_2) # give a parameter
```

### OOP

**Static**

For static method, you need to put `@staticmethod`

```python
class Utilities:
    @staticmethod
    def isRegoValid(rego):
        return true
    
Utilities.isRegoValid(12)
```

**Duck typing**

`Type` of the object is a matter of concern only at runtime.

-   You don't need to explicitly mention the type of object.

```python
class Duck:
   def quack(self):
      print("Quack")

class Flock:
    def __init__(self, ducks):
        self.ducks = ducks

    def report(self):
        print("There are {} ducks in this flock.".format(len(self.ducks)))
        for duck in self.ducks:
            duck.quack()
        print()

flock = Flock([Duck(), Duck(), Duck()])
flock.report()

class Dog:
   def quack(self):
      print("BARK! BARK! BARK!")

flock = Flock([Duck(), Duck(), Dog(), Duck()])
flock.report()
```

>   Dog makes no difference

**Iteration**

```python
class Factorial():
    def __init__(self, value):
        self.value = value

    def __iter__(self):
        self.total = 1
        self.previous = 1
        return self

    def __next__(self):
        if(self.previous > self.value):
            raise StopIteration
        
        self.total *= self.previous
        self.previous += 1

        return self.total

fiveFactorial = Factorial(5)

for value in fiveFactorial:
    print(value)

print("---")

try:
    iterator = iter(fiveFactorial)
    while(True):
        value = next(iterator)
        print(value)
except StopIteration:
    pass
```

>   You need to have `__iter__` and `__next__`

**Genereator**

Let you perform the same operation again with memory efficient manner

-   It contains the keyword `yield`
-   It must be noted that the mere presence of this keyword change the nature of function
    -   This yield statement doesn't have to be invoked or even reachable, but causes the function to be marked as a generator

`yield` is similar to `return`

```python
class Factorial():
    def __init__(self, value):
        self.value = value

    def values(self):
        total = 1
        previous = 1
        while(previous <= self.value):
            total *= previous
            previous += 1
            yield total

fiveFactorial = Factorial(5)

for value in fiveFactorial.values():
    print(value)
```



**Closure**

Combination of a function, a function inside a function.



```python
# Reference: https://www.programiz.com/python-programming/closure
def make_multiplier_of(n):
    def multiplier(x):
        return x * n
    return multiplier #return a function

# Multiplier of 3.
times3 = make_multiplier_of(3)

# Multiplier of 5.
times5 = make_multiplier_of(5)

# Output: 27
print(times3(9))

# Output: 15
print(times5(3))

# Output: 30
print(times5(times3(2)))

print("---")

print(times3.__closure__[0].cell_contents)
print(times5.__closure__[0].cell_contents)
```



In python, the referencing environment is stored as a tuple of cells. You can access them by using the func_closure or `__closure__` attribute





**decorator**

```python
# Reference: https://realpython.com/primer-on-python-decorators/#decorating-classes
def my_decorator(func):
    def wrapper():
        print("--- Before calling the function ---")
        func()
        print("--- After calling the function ---")
    return wrapper

@my_decorator
def hello_world():
    print("Hello World!")

hello_world()

```

```
--- Before calling the function ---
Hello World!
--- After calling the function ---
```

It will run `my_decorator` first

## Data from web

**Cron jobs**

Ref: [ostechnix.com/a-beginners-guide-to-cron-jobs](https://www.ostechnix.com/a-beginners-guide-to-cron-jobs/)

