# 101 Decorators for Python

A comprehensive guide to the usage and implementation of decorators in Python.


## Table of Contents

* Introduction to Decorators
* Classification and Examples
* Specific Use Cases
* Standard Library and Module Usage
* Community Contribution and Expansion

## Introduction to Decorators

Decorators are a powerful and useful feature in Python that allow you to modify the behavior of a function or class. They are a form of metaprogramming, which is the ability to write code that manipulates or generates other code. Decorators are often used to add extra functionality to existing functions, such as adding caching or debugging capabilities.

The purpose of this project is to provide a comprehensive collection of decorators, along with clear and concise explanations and examples. It is intended to serve as a resource for those looking to learn about or improve their understanding of decorators in Python. The project also aims to inspire others to contribute their own decorators and use cases, and to start a conversation about the many possibilities and applications of decorators.

The project is organized into sections, each focusing on a particular aspect or type of decorator. It includes both general and specific examples, as well as examples from popular libraries and modules such as Django and Flask.

We hope that this project will be a useful and informative resource for anyone looking to use decorators in their own projects, and that it will inspire others to explore the full potential of this powerful and useful feature in Python.


## Classification and Examples

Decorators can be classified in several ways, based on their purpose or the way in which they are applied. Some common types of decorators include:

### Function Decorators: 
These decorators are applied to functions and modify their behavior. Examples include `@log` to log function calls, or `@cache` to cache the results of a function for improved performance. 
- Here is an example of a function decorator that prints a message before and after the decorated function is called:

          def print_message(func):
              def wrapper(*args, **kwargs):
                  print("Before function call")
                  result = func(*args, **kwargs)
                  print("After function call")
                  return result
              return wrapper
      
          @print_message
          def add(x, y):
              return x + y
      
          result = add(3, 4)
          print(result)

    Output: 
    
        Before function call
        After function call
        7


### Method Decorators: 
These decorators are applied to methods of a class and can modify their behavior within the context of the class. Examples include `@classmethod` to define a method that operates on the class itself, rather than an instance.

  - Here is an example of a method decorator that prints a message before and after the decorated method is called:

        def print_message(method):
            def wrapper(self, *args, **kwargs):
                print("Before method call")
                result = method(self, *args, **kwargs)
                print("After method call")

        class ExampleClass:
            def __init__(self):
                self.value = 0
      
            @print_message
            def add(self, x):
                self.value += x
            return self.value
      
        example = ExampleClass()
        example.add(5)

      Output: 

          Before method call
          After method call
          
    In this example, the `@print_message` decorator is applied to the add method of the ExampleClass class. When the add method is called, the wrapper function is executed first, which prints the message "Before method call" and then calls the add method. After the add method has finished executing, the message "After method call" is printed.<br>


### Class Decorators: 
These decorators are applied to classes and can modify their behavior or add additional functionality. Examples include `@singleton` to ensure a class only has a single instance, or @register to register a class as a plugin or extension.
  - Here is an example of a class decorator that adds a class attribute to the decorated class:
    
        def add_attribute(cls):
            cls.new_attribute = "This is a new class attribute"
            return cls
          
        @add_attribute
        class MyClass:
            pass
          
        print(MyClass.new_attribute)
  
      Output:
  
        This is a new class attribute
        

### Property Decorators: 
These decorators are applied to properties of a class and can modify how the property is accessed or set. Examples include `@cached_property` to cache the results of a property getter for improved performance, or @restricted to restrict access to the property.
- Here is an example of a property decorator that logs a message when the property is accessed:
            
       def log_access(method):
           def wrapper(self):
               print(f"Accessing property {method.__name__}")
               return method(self)
           return wrapper
            
       class MyClass:
           def __init__(self):
               self._x = 0]()
        
       @property
       @log_access
       def x(self):
           return self._x
            
       @x.setter
       def x(self, value):
           self._x = value
        
  In this example, the `x` property has a getter method decorated with the `@log_access` decorator. When the `x` property is accessed, the decorator will log a message before returning the value of the property. The setter method for the `x` property is also decorated with the `@log_access` decorator, so it will log a message whenever the value of the `x` property is modified.<br>


- Here is another example of a property decorator that enforces a minimum and maximum value for the property:

      def min_max(min_value, max_value):
          def decorator(method):
              def wrapper(self, value):
                  if value < min_value:
                      raise ValueError(f"Value must be greater than or equal to {min_value}")
                  if value > max_value:
                      raise ValueError(f"Value must be less than or equal to {max_value}")
                  return method(self, value)
              return wrapper
          return decorator
        
      class MyClass:
          def __init__(self):
              self._x = 0
            
          @property
          def x(self):
              return self._x
            
          @x.setter
          @min_max(0, 100)
          def x(self, value):
              self._x = value

  In this example, the setter method for the `x` property is decorated with the `@min_max` decorator, which enforces a minimum value of 0 and a maximum value of 100 for the `x` property. If the value being set is outside of this range, the decorator will raise a `ValueError`.


## Debugging Decorators

When working with decorators, it can be helpful to use tools and techniques that allow you to introspect and debug them. Here are some approaches you can use to debug decorators in Python:

* Use the `@functools.wraps` decorator: When you define a decorator, the decorated function loses its original identity, including its name and docstring. You can use the `@functools.wraps` decorator to preserve the decorated function's identity and make it easier to introspect and debug. Here is an example of how to use `functools.wraps`:

        from functools import wraps

        def greet(func):
            @wraps(func)
            def wrapper(name):
                print(f"Hello, {name}")
                func()
                print("!")
            return wrapper

* Use the `__name__` attribute: You can use the `__name__` attribute to access the name of a function, even if it has been decorated. For example:

        def greet(func):
            def wrapper(name):
                print(f"Hello, {name}")
                func()
                print("!")
            return wrapper

        @greet
        def say_hello():
            print('hello')
            
        print(say_hello.__name__)  # Output: wrapper

* Use the `inspect` module: The `inspect` module provides several functions that allow you to introspect the properties of objects in Python, including functions. For example, you can use the `inspect.signature` function to access the signature (i.e., the arguments and return type) of a function, even if it has been decorated:

        import inspect

        def greet(func):
            def wrapper(name):
                print(f"Hello, {name}")
                func()
                print("!")
            return wrapper

        @greet
        def say_hello():
            print('hello')
            
        sig = inspect.signature(say_hello)
        print(sig)  # Output: (name)


It is important to make sure that decorators do not interfere with the traceback information when an error occurs. A traceback is the record of the sequence of function calls that led to an error, and it is an essential tool for debugging and troubleshooting code. If a decorator hides or modifies the traceback information, it can make it much more difficult to understand and fix the root cause of the error. To avoid this problem, you should make sure that your decorators do not interfere with the traceback information. Here are some best practices to follow:

* Use the `@functools.wraps` decorator: As mentioned earlier, the `@functools.wraps` decorator preserves the decorated function's identity and makes it easier to introspect and debug. This includes preserving the function's traceback information.

* Do not modify the decorated function's code: Avoid making changes to the decorated function's code that could affect its traceback information. For example, do not add or remove lines of code, or change the function's arguments or return type.

* Use the `raise` statement to propagate errors: When an error occurs in the decorated function, use the `raise` statement to propagate the error to the caller, rather than handling the error within the decorator. This will preserve the original traceback information.

By following these best practices, you can ensure that your decorators do not interfere with the traceback information and make it easier to debug and troubleshoot errors in your code.<br>

## Specific Examples

In addition to the general categories listed above, this project will also include specific examples of decorators for common use cases. These may include:

* Validation: Decorators to validate input or output of a function, such as @validate_json to ensure a function only receives valid JSON input.
* Error Handling: Decorators to handle errors or exceptions thrown by a function, such as @retry to retry a function upon failure.
* Performance: Decorators to improve the performance of a function, such as @lru_cache to implement a least-recently-used cache.
* Concurrency: Decorators to add concurrency or asynchronous behavior to a function, such as @asyncio.coroutine to define an asynchronous generator function.

## Standard Library and Module Usage

Decorators are used extensively in popular libraries and modules, such as Django and Flask for web development, or asyncio and threading for concurrent programming.

* In Django, decorators are used for routing and user authorization. For example, the @login_required decorator can be used to ensure that a user is authenticated before they are allowed to access a certain view or route. In Flask, the @app.route decorator is used to define the routes for a web application.

* Asyncio and threading both use decorators to specify which functions should be run concurrently. For example, the @asyncio.coroutine decorator is used to specify that a function should be run asynchronously, while the @threading.thread decorator is used to specify that a function should be run in a separate thread.

* Overall, decorators are a powerful tool for extending the functionality of functions and classes in a clean and concise manner. They can be used in a wide variety of contexts, from web development to concurrent programming, and are an essential part of any Python programmer's toolkit.