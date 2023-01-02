# 101 Decorators for Python

A comprehensive guide to the usage and implementation of decorators in Python.
Table of Contents

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
These decorators are applied to functions and modify their behavior. Examples include @log to log function calls, or @cache to cache the results of a function for improved performance. 
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
<br>

### Method Decorators: 
These decorators are applied to methods of a class and can modify their behavior within the context of the class. Examples include @classmethod to define a method that operates on the class itself, rather than an instance.

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
          
    In this example, the @print_message decorator is applied to the add method of the ExampleClass class. When the add method is called, the wrapper function is executed first, which prints the message "Before method call" and then calls the add method. After the add method has finished executing, the message "After method call" is printed.<br>


### Class Decorators: 
These decorators are applied to classes and can modify their behavior or add additional functionality. Examples include @singleton to ensure a class only has a single instance, or @register to register a class as a plugin or extension.
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
These decorators are applied to properties of a class and can modify how the property is accessed or set. Examples include @cached_property to cache the results of a property getter for improved performance, or @restricted to restrict access to the property.
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
        
  In this example, the x property has a getter method decorated with the log_access decorator. When the x property is accessed, the decorator will log a message before returning the value of the property. The setter method for the x property is also decorated with the log_access decorator, so it will log a message whenever the value of the x property is modified.<br>


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

  In this example, the setter method for the x property is decorated with the min_max decorator, which enforces a minimum value of 0 and a maximum value of 100 for the x property. If the value being set is outside of this range, the decorator will raise a ValueError.
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