# Week05: Data from web (API), python advanced features, bluetooth and raspberry pi

## Segment1. RESTful

-   REST = Representational State Transfor
-   RESTful design = a style of web architecture
-   Takes the advantage of HTTP when used for Web APIs
    -   This means that developers do not need to install libraries or additional software in order to take advantage of a REST API design

!> Since data is not tied to methods and resources, REST has the ability to handle multiple types of calls, return different data formats.



-   A REST API
    -   defines a set of functions which developers can perform GET requests and POST.
-   Client and Server are independent of each other
-   No client data is stored on the server between requests and session state is stored on the client.



Some of the method:

-   GET, POST, PUT, DELETE



## Python

### Design patterns

-   Design Patterns provide standardised and efficient solutions to software design and programming problems that are re-usable in your code.
-   Centred around many object oriented axioms:
    -   Cohesion / Coupling
    -   Encapsulation

### Singleton Pattern

-   Restricts the instantiation of a class to one object
-   It is a type of creational pattern and involves only one class to create methods and specified objects

```python
# Reference: https://www.tutorialspoint.com/python_design_patterns/python_design_patterns_singleton.htm
class Singleton:
    __instance = None

    @staticmethod
    def getInstance():
        if(Singleton.__instance == None):
            Singleton()
        return Singleton.__instance

    def __init__(self):
        if(Singleton.__instance != None):
            raise Exception("This class is a singleton!")
        else:
            Singleton.__instance = self

s = Singleton()
print(s)

s = Singleton.getInstance()
print(s)

s = Singleton.getInstance()
print(s)
```

### Factory pattern

-   Objects are created without exposing the logic to client and referring to the newly created object using a common interface.

```python
# Reference: https://www.tutorialspoint.com/python_design_patterns/python_design_patterns_factory.htm
class Button():
    def __init__(self):
        self.html = ""
    
    def get_html(self):
        return self.html

class Image(Button):
    def __init__(self):
        super().__init__()
        self.html = "<img></img>"

class Input(Button):
    def __init__(self):
        super().__init__()
        self.html = "<input></input>"

class Flash(Button):
    def __init__(self):
        super().__init__()
        self.html = "<obj></obj>"

class ButtonFactory():
    def create_button(self, type):
        target_class = type.capitalize()
        return globals()[target_class]()

button_factory = ButtonFactory()
button = ["image", "input", "flash"]
for b in button:
    print(button_factory.create_button(b).get_html())
```

Everything is extended from `Button` and by calling `get_html()` it will return the 

## Builder pattern

Builder Pattern is unique design pattern which helps in building complex object using simple objects and uses an algorithmic approach

In this design pattern, a builder class builds the final object in step - step procedure. This builder is independent of other objects



```python
# Reference: https:#code.msdn.microsoft.com/windowsapps/Design-Patterns-Builder-26d38c45
# Adapted into python code.
from abc import ABC, abstractmethod

# Product - The main object that will be created by the Director using Builder.
class Pizza:
    def __init__(self):
        self.sauce = ""
        self.topping = ""

# Builder - Abstract interface for creating objects (the product).
class PizzaBuilder(ABC):
    def __init__(self):
        self._pizza = None

    def GetPizza(self):
        return self._pizza
    
    def CreateNewPizzaProduct(self):
        self._pizza = Pizza()

    @abstractmethod
    def BuildSauce(self):
        pass
    
    @abstractmethod
    def BuildTopping(self):
        pass

# Concrete Builder - Provides implementation for Builder An object able to construct other objects.
# Constructs and assembles parts to build the objects.
class CheesePizzaBuilder(PizzaBuilder):
    def __init__(self):
        super().__init__()

    def BuildSauce(self):
        self._pizza.Sauce = "Cheese"

    def BuildTopping(self):
        self._pizza.Topping = "Green Pepper"

# Concrete Builder - Provides implementation for Builder An object able to construct other objects.
# Constructs and assembles parts to build the objects.
class SpicyPizzaBuilder(PizzaBuilder):
    def __init__(self):
        super().__init__()
    
    def BuildSauce(self):
        self._pizza.Sauce = "Hot Sauce"

    def BuildTopping(self):
        self._pizza.Topping = "Red Pepper, Jalapeno"

# Director - Responsible for managing the correct sequence of object creation.
# Receives a Concrete Builder as a parameter and performs the necessary operations on it.
class Cook:
    def __init__(self):
        self.__pizzaBuilder = None

    # Step 1. Construct the builder object for the given concrete object.
    def SetPizzaBuilder(self, pizzaBuilderObject):
        self.__pizzaBuilder = pizzaBuilderObject

    # Step 2. Call required methods from the builder class.
    # Step 3. Call required methods from the concrete builder classes.
    def ConstructPizza(self):
        # Calling builder object's method.
        self.__pizzaBuilder.CreateNewPizzaProduct()

        # Calling Concrete object's methods.
        self.__pizzaBuilder.BuildSauce()
        self.__pizzaBuilder.BuildTopping()

    # Step 4. Return the product after creating it based on the builder and concrete objects.
    def GetPizza(self):
        return self.__pizzaBuilder.GetPizza()

# 1. Create an instance for Builder and initialize with Concrete builder - CheesePizzaBuilder.
cheesePizzaBuilder = CheesePizzaBuilder()

# 2. Create an instance for Director.
cook = Cook()

# 3. By using Director instance, call the concrete object methods.
cook.SetPizzaBuilder(cheesePizzaBuilder)
cook.ConstructPizza()

# 4. Create and get the product, by using the Director.
cheesePizzaProduct = cook.GetPizza()

# 5. Deliver the product.
print("Cheese Pizza prepared by using {} and {}".format(cheesePizzaProduct.Topping, cheesePizzaProduct.Sauce))

# 1. Create an instance for Builder and initialize with Concrete builder - SpicyPizzaBuilder.
spicyPizzaBuilder = SpicyPizzaBuilder()

# 2. Use same Director instance.
# 3. And pass the new concrete Builder instance.
cook.SetPizzaBuilder(spicyPizzaBuilder)
cook.ConstructPizza()

# 4. Create and get the product, by using the Director.
spicyPizzaProduct = cook.GetPizza()

# 5. Deliver the product.
print("Spicy Pizza prepared by using {} and {}".format(spicyPizzaProduct.Topping, spicyPizzaProduct.Sauce))
```

PizzaBuilder will initialise the pizza. Based on what the builder you specify, it wil have a different `BuildSauce` and `BuildTopping` method.

 ## Adapter pattern

Adapter pattern works as a bridge between two incompatible interfaces.

This pattern involves a single class, which is responsible to join functionalities of independent or incompatible interfaces

The adapter design pattern helps to work classes together. It converts the interface of a class into another interface based on requirement.

```python
# Reference: http://msdn.microsoft.com/en-us/library/orm-9780596527730-01-04.aspx
# Adapted into python code. :)
from abc import ABC, abstractmethod

# Simplest adapter using interfaces and inheritance.

# Code Base X -> old (legacy).
# Code Base Y calls into Code Base X.
# New Code will only call Code Base Y.

# Existing way requests are implemented.
class Adaptee:
    # Provide full precision.
    def SpecificRequest(self, a, b):
        return a / b

# Required standard for requests.
class ITarget(ABC):
    # Rough estimate required.
    @abstractmethod
    def Request(self, i):
        pass

# Implementing the required standard via Adaptee.
class Adapter(Adaptee, ITarget):
    def Request(self, i):
        return "Rough estimate is {}".format(round(self.SpecificRequest(i, 3)))

# Showing the Adaptee in standalone mode - legacy code.
first = Adaptee()
print("Before the new standard.")
print("Precise reading: {}".format(first.SpecificRequest(5, 3)))

# New code.
second = Adapter()
print()
print("Moving to the new standard.")
print(second.Request(5))
```

So you extends 2 classes. That's it.

## Bluetooth

Is a wireless technology standard for exchanging data over short distances from fixed and mobile devices

Frequency: **2.4 GHz**

Bluetooth devices automatically detect and connect to one another and up to 8 of them can communicate at any one time

They do not interfere with one another because each pair of devices uses a different one of the **79 available channels**

### Bluetooth & R Pi

R Pi kit comes with on board built-in Bluetooth support.

>   Details: https://www.element14.com/community/docs/DOC-81266/l/setting-up-bluetooth-on-theraspberry-pi-3?CMP=SOM-TWITTER-PRG-BLOGCSTANTON-BTPI3

Commands to get started

```bash
bluetoothctl
```

Then run

```python
power on
agent on
scan on
```

### Bluetooth, R Pi and Python

-   Pybluez is the Bluetooth Python extension

-   You first will need to install it in the Raspian

    ```bash
    sudo pip install pybluez
    ```

-   Example code

    ```python
    # Simple inquiry example.
    # 
    # sudo apt install bluetooth bluez python-bluez blueman
    # pip3 install pybluez
    import bluetooth
    
    nearby_devices = bluetooth.discover_devices(lookup_names = True)
    print("Found %d devices" % len(nearby_devices))
    
    for addr, name in nearby_devices:
        print("%s - '%s'" % (addr, name))
    ```

    List paired devices:

    ```python
    import subprocess as sp
    import os
    
    # To pair devices see: https://www.cnet.com/how-to/how-to-setup-bluetooth-on-a-raspberry-pi-3/
    # sudo apt install bluetooth bluez blueman
    # pip3 install pybluez
    # 
    # To use bt-device: sudo apt install bluez-tools
    
    # Get list of paired devices.
    p = sp.Popen(["bt-device", "--list"], stdin = sp.PIPE, stdout = sp.PIPE, close_fds = True)
    (stdout, stdin) = (p.stdout, p.stdin)
    
    data = stdout.readlines()
    print(data)
    ```



# Documentation

Generating documentation of your code.



## What documentation?

To document your stuff for other people to understand how it works, how they can use it.

Python allows you to add information about your program using `docstrings`

`Docstrings` are the basis for creating documentation for your code.

For example:

```python
def helloworld():
    """
    This function prints 'hello world' to the screen.
    It accepts no arguments and returns nothing
    """
    print("hello world")
```



To generate the doc, run

```bash
python -m pydoc -w Name_of_python_file
```

