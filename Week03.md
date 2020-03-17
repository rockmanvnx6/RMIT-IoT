# Week03 Database Programming, Sense HAT Programming for RBPI

## Python3: Object oriented

In python we have

-   `private`, `protected`
-   `abstract`



### Underscore pattern

1.  Single Leading Underscore:

     `_var`

2.  Single Trailing Undercore:

     `var_` you already have a name `var` but you want to have another `var`

3.  Double leading underscore:

    `__var`: causes python interpreter to rewrite the attribute name to avoid conflict.

4.  Double Leading and trailing: 

	`__var__`: reserved for special use. For example:

    `__init__` for constructor or make the object callable `__call__`
    
    

5.  Single Underscore: 

    `_` temperary name, don't need to give a proper name

### Private, protected

To make a variable **protected**, it will be protected: `_variableName`

Too make it **private**, use a double prefix underscore: `__variableName`



Example:

```python
class employee:
    def __init__(self, name, sal):
        self.__name=name  # private attribute 
        self.__salary=sal # private attribute

e1=employee("Bill",10000)
e1.__salary # cannot access because it's outside of the class
```

In order to access it, it has to be `_salary` or `salary`



### ABC (Abstract Base Class)

```python
# you will need a module ABC

from abc import ABC, abstractmethod
 
 
class AbstractOperation(ABC):
 
    def __init__(self, operand_a, operand_b):
        self.operand_a = operand_a
        self.operand_b = operand_b
        super(AbstractOperation, self).__init__()
    
    # this is called as a decorator

    @abstractmethod
    def execute(self):
        pass

# children

class AddOperation(AbstractOperation):
    def execute(self):
        return self.operand_a + self.operand_b
 
 
class SubtractOperation(AbstractOperation):
    def execute(self):
        return self.operand_a - self.operand_b
 
 
class MultiplyOperation(AbstractOperation):
    def execute(self):
        return self.operand_a * self.operand_b
 
 
class DivideOperation(AbstractOperation):
    def execute(self):
        return self.operand_a / self.operand_b

# now you can write some code to print values
```

## Python3: String

?> We use both single and double quote to define string

`[start,end,increment]`

```python
# declare strings and access sub strings
astring = "Hello world!"
print(astring[3:7])

# reverse a string
astring = "Hello world!"
print(astring[::-1])

# multiple string operations
s = "Strings are awesome!"
# Length should be 20
print("Length of s = %d" % len(s))

# First occurrence of "a" should be at index 8
print("The first occurrence of the letter a = %d" % s.index("a"))

# Number of a's should be 2
print("a occurs %d times" % s.count("a"))

# Slicing the string into bits
print("The first five characters are '%s'" % s[:5]) # Start to 5
print("The next five characters are '%s'" % s[5:10]) # 5 to 10
print("The thirteenth character is '%s'" % s[12]) # Just number 12
print("The characters with odd index are '%s'" %s[1::2]) #(0-based indexing) print
print("The last five characters are '%s'" % s[-5:]) # 5th-from-last to end

# Convert everything to uppercase
print("String in uppercase: %s" % s.upper())

# Convert everything to lowercase
print("String in lowercase: %s" % s.lower())

# Check how a string starts
if s.startswith("Str"):
    print("String starts with 'Str'. Good!")

# Check how a string ends
if s.endswith("ome!"):
    print("String ends with 'ome!'. Good!")

# Split the string into three separate strings,
# each containing only a word
print("Split the words of the string: %s" % s.split(" "))

# what will these print?

print("single quotes are ' '")
print('single quotes are ' '')
```



## RBPI: Reading from internet

Use `urllib` module allows you access a website.

**Note**:

-   Data will be a mess depending on what is returned
-   You will need to use regular expressions to filter what is required
-   It is usually not a good idea for your program to be scrapping data from any website, sometimes they block you.

## Linting

Some tools:

-   PyLint, 
-   PyChecker, 
-   Pyflakes
-   pep8

### Linting: pep8 and pyflakes

-   First need to install flake8

    ```bash
    pip install flake8
    ```

-   Run command for linting

    ```
    flake8 file.py
    pyflakes file.py
    ```

    

## Cron Job

It's scheduled tasks on Unix system

Comamnd: `crontab` (cron table) is used to edit the list of scheduled tasks in operation and is done on a per-user basis.





# Database programming

This course covereing 2 types:

-   Sqlite3
-   MySQL

!> Bad written sql can trigger SQL Injection attacks

MySQL is more scalable, can be tuned more easily, which supports user management and permission.

MYSQL can be used in client server architectures where a database client must access a database remotely



## Python programming for GPIO

?> **GPIO** interface enables one to connect the raspberry pi to electronic circuits and then interact with outside world

Original RbPI **A and B** used a **26-pin** interface, whereas the **model B+** and **RbPI 2/3/4** provide a **40 pin interface**

Boardcom chip includes

-   Multiple digital input/output (I/O) pins
-   Pulse-width modulation (PWM) output
-   Inter-integrated circuit (I2C) interface.
-   Serial Periphral Interface (SPI) interface
-   Universal Asynchronous Receiver/Transmitter (UART)



## SenseHAT

**1. Display messages**

```python
from sense_hat import SenseHat
sense = SenseHat()
sense.show_message("Hello world")

# show colour

r = 255
g = 0
b = 0

sense.clear((r, g, b))

# setting single pixels

sense.set_pixel(2, 2, (0, 0, 255))
sense.set_pixel(4, 2, (0, 0, 255))
sense.set_pixel(3, 4, (100, 0, 0))
sense.set_pixel(1, 5, (255, 0, 0))
sense.set_pixel(2, 6, (255, 0, 0))
sense.set_pixel(3, 6, (255, 0, 0))
sense.set_pixel(4, 6, (255, 0, 0))
sense.set_pixel(5, 5, (255, 0, 0))

# more on display

blue = (0, 0, 255)
yellow = (255, 255, 0)

while True:
  sense.show_message("Astro Pi is awesome!", text_colour=yellow, back_colour=blue, scroll_speed=0.05)

```



**2. Using Sensor**

```python
# Reading pressure with the Sense HAT

from sense_hat import SenseHat

sense = SenseHat()
sense.clear()

pressure = sense.get_pressure()
print(pressure)

# temperature

sense.clear()

temp = sense.get_temperature()
print(temp)

# humidity

sense.clear()

humidity = sense.get_humidity()
print(humidity)
```



```python
from sense_hat import SenseHat

sense = SenseHat()

red = (255, 0, 0)

while True:
    acceleration = sense.get_accelerometer_raw()
    x = acceleration['x']
    y = acceleration['y']
    z = acceleration['z']

    x = abs(x)
    y = abs(y)
    z = abs(z)

    if x > 1 or y > 1 or z > 1:
        sense.show_letter("!", red)
    else:
        sense.clear()
```



**3. Button**

```python
from sense_hat import SenseHat
from time import sleep
sense = SenseHat()

e = (0, 0, 0)
w = (255, 255, 255)

sense.clear()
while True:
  for event in sense.stick.get_events():
    # Check if the joystick was pressed
    if event.action == "pressed":
      
      # Check which direction
      if event.direction == "up":
        sense.show_letter("U")      # Up arrow
      elif event.direction == "down":
        sense.show_letter("D")      # Down arrow
      elif event.direction == "left": 
        sense.show_letter("L")      # Left arrow
      elif event.direction == "right":
        sense.show_letter("R")      # Right arrow
      elif event.direction == "middle":
        sense.show_letter("M")      # Enter key
      
      # Wait a while and then clear the screen
      sleep(0.5)
      sense.clear()
```

