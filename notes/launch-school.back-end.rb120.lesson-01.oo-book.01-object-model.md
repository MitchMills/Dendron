---
id: 2ky0opk5l06bm34930pdx51
title: 01 Object Model
desc: ''
updated: 1721643008941
created: 1721317430358
---
## Why Object Oriented Programming?
### Object Oriented Programming (OOP)
- programming paradigm
  - created to deal with growing complexity of large software systems
  - as programs grow in complexity and size:
    - they become very difficult to maintain
    - dependencies throughout a program mean any changes or bugs can affect entire program

  - OOP is a way to create containers for data so that:
    - data can be change and manipulated without affecting the entire program
    - programs become the interaction of many small parts
  instead of one massive blob of dependencies
### OOP Terminology
#### Encapsulation
- bundling the data and operations that work on that data into a single entity (object)
  - i.e. combining state (data) and behavior (operations) together to form an object
- encapsulation hides functionality:
  - makes it unavailable to rest of code base
  - as a result, data cannot be manipulated or changed without obvious intent
- Ruby accomplishes encapsulation via creating objects and exposing interfaces (methods) to interact with those objects
- Broader purpose: restricts access to state and certain behaviors of objects
  - an object only exposes the data and behaviors that other parts of the application need to work
  - objects expose a **public interface** for interacting with other objects
  - objects keep their implementation details hidden
  - therefore other objects can't change the data of an object without going through the proper interface
#### Polymorphism
- the ability for different types of data to respond to a common interface
  - if `this_method` invokes the `#do_this` method on its argument, we can pass `this_method` any type of argument as long as that argument has a compatible `#do_this` method.
  - objects of different types can respond to the same method invocation
#### Inheritance
- a class inherits (acquires) the behaviors of another class, referred to as the **superclass**
  - this allows defining basic classes with large reusability
    - and smaller **subclasses** for more fine-grained, detailed behaviors
- a `Module` is similar to a class in that it contains shared behavior.
  - however, an object cannot be created with a module
  - for a class and its objects to be able to use a module's behavior:
    - the module must be mixed in with the class using the `#include` method invocation
    - this is called a **mixin**
## What Are Objects?
- objects are created from classes
  - individual objects are instances of their class
  - individual objects can contain different information from other objects of the same class
    - for example, these are two instances of `String` class objects:
      ```ruby
      string1 = 'hello'
      string2 = 'world'
      ```
### Classes Define Objects
- ruby defines the attributes and behaviors of its objects in **classes**
- classes: basic outlines of what an object should be made of and what it should be able to do
- syntax for defining a class:
  - use CamelCase for the name
  - use the `class` reserved word
    ```ruby
    class GoodDog
      # attributes and behaviors
    end

    debs = GoodDog.new
    ```
  - the above code created an instance of the `GoodDog` class and stored it in the variable `debs`.
    - this instance is an object
      - "`debs` is an object or instance of class `GoodDog`."
    - creating an instance from a class is called **instantiation**
      - "we have instantiated an object called `debs` from the class `GoodDog`."
    - an object is returned by calling the class method `#new`
### Modules
- a **module** is a collection of behaviors that is usuable in other classes via **mixins**
  - a module is "mixed in" to a class using the `Module#include` method invocation.
  - Example:
    ```ruby
    module Speak
      def speak(sound)
        puts sound
      end
    end

    class GoodDog
      include Speak
    end

    class HumanBeing
      include Speak
    end

    sparky = GoodDog.new
    sparky.speak("Arf!")  # => Arf!
    bob = HumanBeing.new
    bob.speak("Hello!")   # => Hello!
    ```
    - both `sparky` (a `GoodDog` object) and `bob` (a `HumanBeing` object) have access to the `#speak` instance method because their respective classes both mixed in the `Speak` module.
### Method Lookup
- when a method is invoked, ruby uses a distinct lookup path to determine what to do.
  - the `Module#ancestors` method can be used on any class to find out the method lookup chain
- Example:
    ```ruby
    module Speak
      def speak(sound)
        puts "#{sound}"
      end
    end

    class GoodDog
      include Speak
    end

    puts GoodDog.ancestors
    ```
- Output:
    ```ruby
    GoodDog
    Speak
    Object
    Kernel
    BasicObject
    ```
- ruby starts looking for the invoked method at the top of the list, and moves down until the method is either found, or there are no more places to look