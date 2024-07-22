---
id: yh0v7rtu6siz8vov27trup2
title: 02 Classes and Objects 1
desc: ''
updated: 1721643816462
created: 1721334670575
---
## States and Behaviors
- Classes are used to create objects
- When defining a class, we usually focus on:
  - state
    - data associated to an individual object
      - tracked by instance variables
        - scoped at the object (instance) level
  - behaviors
    - what objects of the class are capable of doing
      - defined as instance methods
        - availble to objects / instances of that class
## Initializing a New Object
- an `#initialize` method can be included in the class definition:
  ```ruby
  class GoodDog
    def initialize
      puts "This object was initialized!"
    end
  end

  sparky = GoodDog.new  # => "This object was initialized!"
  ```
- This `#initialize` method gets called every time a new object of that class is created
- Instantiating a new class object using `#new` triggers the `#initialize` method
- `#initialize` is called a *constructor*
  - it is a special method that builds the object when a new class object is instantiated
## Instance Variables
- Example of defining a class so that it can create a new object and instantiate it with some state:
  ```ruby
  class GoodDog
    def initialize(name)
      @name = name
    end
  end
  ```
- `@name` is an **instance variable**
  - an instance variable exists as long as the instance of the object exists
  - it is one of the ways to tie data to objects
  - defining an instance method with parameter(s) allows arguments to be passed in
    - in the example above, the `name` parameter allows an argument to be passed in to the `#initialize` method (via the `#new` method)
- Example: creating a new object of the `GoodDog` class and passing in an argument:
  ```ruby
  sparky = GoodDog.new("Sparky")
  ```
  - The string object `'Sparky'` is passed from the `#new` method through to the `#initialize` method, and assigned to the local variable `name`
  - Then within the constructor (i.e. the `#initialize` method), the instance variable `@name` is set to `name`
  - This results in assigning the string `'Sparky'` to the `@name` instance variable
- Instance variables are responsible for keeping track of information about the *state* of an object
  - in the example above, the object referenced by local variable `sparky` has a state value of the string `'Sparky'`, which is tracked in the instance variable `@name`.
- every object's state is distinct, even if it is of the same class as another object
- instance variables are how we keep track of these distinct states
### Composition and Aggregation
- In OOP languages, **composition** and **aggregation** are design principles
  - they are used to establish relationships between classes
  - both involve using instance variables to hold references to other objects
    - they differ in terms of the lifecycle and ownership of the objects involved
#### Composition
- A relationship where an object contains one or more objects of other classes as part of its state
  - the outer object is often called **the container**
  - the inner objects are often called **contained objects** or **composed objects**
- The contained / composed objects are tied directly to the container
  - the lifetimes of the container and composed objects are dependent on each other
  - the composed objects are created when the container is created and destroyed when the container is destroyed
- In ruby, composition is typically implemented using instance variables that are initialized via the constructor of the class:
  ```ruby
  class Engine
    def start
      puts "Engine starting..."
    end
  end

  class Car
    def initialize
      @engine = Engine.new  # Engine instance is created when Car is created
    end

    def start
      @engine.start
    end
  end

  my_car = Car.new
  my_car.start  # Engine is an integral part of Car
  ```
  - In this example, `Car` has an `Engine`, and instances of `Car` contain `Engine` objects
  - When a new instance of `Car` is instantiated, a new instance of `Engine` is also instantiated.
  - When a `Car` object is destroyed, the composed `Engine` object is also destroyed.
- A container has a "**has-a relationship**" to its composed objects. I.e., the container "has a" composed object.
#### Aggregation
- A form of association that is less tightly coupled than composition
  - the lifetime of an aggregated contained object does not depend on the lifetime of the container
  - the container may have a reference to the contained object, and it may coordinate that object's operations, but that object typically exists independently of the container
- Example:
  ```ruby
  class Passenger
  end

  class Car
    def initialize(passengers)
      @passengers = passengers  # Passengers are given to the Car at creation
    end
  end

  # Passengers can exist without Car
  passengers = [Passenger.new, Passenger.new]
  my_car = Car.new(passengers)
  ```
  - instances of `Car` have a list of `Passenger` objects
    - these `Passenger` objects can exist independently of the `Car` instance.
  - `Passenger` objects can be passed to the `Car` object when it is instantiated, or any time before the `Car` instance is destroyed
  - `Passenger` instances will continue to exist after the `Car` object is destroyed
- A container has a "**has-a relationship**" to its aggregated objects. I.e., the container "has an" aggregated object.
### Relationship to Instance Variables
- The relationships between a container class instance and its composed and aggregated objects is implemented with instance variables
- These instance variables hold references to other objects
  - this allows the container class to access and interact with the methods and properties of the contained objects
- Composition: the container owns the contained objects, and their lifecycles are tightly linked
- Aggregation: the container does not own the contained objects; they can exist independently
- These concepts allow the design of modular systems, where changes to one part of the system do not unintentionally affect others
## Instance Methods
- Adding a behavior (instance method) to the `GoodDog` class:
  ```ruby
  class GoodDog
    def initialize(name)
      @name = name
    end

    def speak
      "Arf!"
    end
  end
  ```
- Creating an instance of `GoodDog` and invoking an instance method on it:
  ```ruby
  sparky = GoodDog.new("Sparky")
  puts sparky.speak  # => Arf!
  ```
- Creating another instance of `GoodDog` and invoking the same method on it:
  ```ruby
  fido = GoodDog.new("Fido")
  puts fido.speak  # => Arf!
  ```
- All objects of the same class can access the same behaviors, though they contain different states
  - in these two examples the differing state is the name
- Instance methods have access to the instance variables defined in their class. For example:
  ```ruby
  def speak
    "#{@name} says arf!"
  end
  ```
This functionality allows instance methods to expose information about the state of the object:
  ```ruby
  puts sparky.speak  # => "Sparky says arf!"
  puts fido.speak    # => "Fido says arf!"
  ```
## Accessor Methods
- If we run this code:
  ```ruby
  puts sparky.name
  ```
- the result is an error:
  ```ruby
  NoMethodError: undefined method `name' for #<GoodDog:0x007f91821239d0 @name="Sparky">
  ```
- This means that the invoked method does not exist or is unavailable to the object that called it.
- The object's name is stored in the `@name` instance variable.
- To access it, we have to create a method that will return the name:
  ```ruby
  class GoodDog
    def initialize(name)
      @name = name
    end

    def get_name
      @name
    end

    def speak
      "#{@name} says arf!"
    end
  end

  sparky = GoodDog.new("Sparky")
  puts sparky.speak     # => Sparky says arf!
  puts sparky.get_name  # => Sparky
  ```
- This is called a **getter** method
- To change state, we can use a **setter** method:
  ```ruby
  class GoodDog
    def initialize(name)
      @name = name
    end

    def get_name # getter method
      @name
    end

    def set_name=(name) # setter method
      @name = name
    end

    def speak
      "#{@name} says arf!"
    end
  end

  sparky = GoodDog.new("Sparky")
  puts sparky.speak             # => Sparky says arf!
  puts sparky.get_name          # => Sparky
  sparky.set_name = "Spartacus"
  puts sparky.get_name          # => Spartacus
  puts sparky.speak             # => Spartacus says arf!
  ```
- The setter method is named `#set_name=`. That special `method_name=` syntax allows two ways of writing the method invocation:
  ```ruby
  sparky.set_name=("Spartacus") # regular
  sparky.set_name = "Spartacus" # syntactical sugar
  ```
- By convention, getter and setter methods are named using the same name as the instance variable they are exposing and setting:
  ```ruby
  class GoodDog
    def initialize(name)
      @name = name
    end

    def name      # This was renamed from "get_name"
      @name
    end

    def name=(n)  # This was renamed from "set_name="
      @name = n
    end

    def speak
      "#{@name} says arf!"
    end
  end

  sparky = GoodDog.new("Sparky")
  puts sparky.speak         # => "Sparky says arf!"
  puts sparky.name          # => "Sparky"
  sparky.name = "Spartacus" # => "Spartacus" (returns passed-in value)
  puts sparky.name          # => "Spartacus"
  puts sparky.speak         # => "Spartacus says arf!"
  ```
- NB: setter methods ALWAYS return the value that is passed in as an argument, regardless of what happens inside the method.
- Code that would normally return something other than the argument's value is ignored:
  ```ruby
  class Dog
    def name=(n)
      @name = n
      "Laddieboy"  # value will be ignored
    end
  end

  sparky = Dog.new()
  puts(sparky.name = "Sparky")  # returns "Sparky"
  ```
- Because *getter* and *setter* methods are so commonplace, ruby has a built-in way to automatically create them, using the `Module#attr_accessor` method:
  ```ruby
  class GoodDog
    attr_accessor :name

    def initialize(name)
      @name = name
    end

    def speak
      "#{@name} says arf!"
    end
  end

  sparky = GoodDog.new("Sparky")
  puts sparky.speak           # => "Sparky says arf!"
  puts sparky.name            # => "Sparky"
  sparky.name = "Spartacus"   # => "Spartacus"
  puts sparky.name            # => "Spartacus"
  puts sparky.speak           # => "Spartacus says arf!"
  ```
- The `#attr_accessor` method takes a symbol as an argument. It uses this to create the method name for *both* the `getter` and `setter` methods.
- The `#attr_reader` method works the same way but only allows retrieval of the instance variable, i.e. it only creates a `getter` method.
- The `#attr_writer` method works the same way but only allows changing the instance variable, i.e. it only creates a `setter` method.
- All the `attr_*` methods take `Symbol` objects as arguments. They can all accept multiple arguments:
  ```ruby
  attr_accessor :name, :height, :weight
  ```
### Accessor Methods in Action
- `getter` and `setter` methods allow us to expose and change an object's state.
- These methods can be used from within the class as well. In this example from the previous section, the `#speak` method references the `@name` instance variable:
  ```ruby
  def speak
    "#{@name} says arf!"  # variable reference
  end
  ```
- Instead of referencing the instance variable directly (`@name`), we can use the `name` getter method created by `#attr_accessor`:
  ```ruby
  def speak
    "#{name} says arf!"  # method invocation
  end
  ```
- Referencing the instance variable and calling the instance method both work in this case. However, callling the getter method is better practice.
- Example:
  - we are keeping track of social security numbers in instance variable `@ssn`.
  - we don't want to expose the raw data (i.e. the entire SSN) in our application.
  - when we retrieve the data, we only want to display the last 4 digits and mask the rest: `xxx-xx-1234`
  - if we reference the `@ssn` instance variable directly, we then need to sprinkle our entire class with code like:
    ```ruby
    # converts '123-45-6789' to 'xxx-xx-6789'
    'xxx-xx-' + @ssn.split('-').last
    ```
  - it is easier to define the getter method to do this, so that the formatting (and any bug fixes or changes) only happen in one place:
    ```ruby
    def ssn
      'xxx-xx-' + @ssn.split('-').last
    end
    ```
  - this way the `ssn` instance method can be used throughout the class to retrieve the already-formatted SSN.
- This also holds true for the setter method. Any time the instance variable is being changed directly in the class, it's better to use the setter method instead of referencing the instance variable itself.
  - Example:
    - we add more states to the `GoodDog` class:
      ```ruby
      attr_accessor :name, :height, :weight
      ```
    - this one line of code produces six getter/setter instance methods (`name`, `name=`, `height`, `height=`, `weight`, `weight=`)
    - it also produces three instance variables (`@name`, `@height`, `@weight`)
    - now we want to create a new method that allows changing several states at once: `change_info(n, h, w)`:
      ```ruby
      def change_info(n, h, w)
        @name = n
        @height = h
        @weight = w
      end
      ```
    - our `GoodDog` class definition now looks like this:
      ```ruby
      class GoodDog
        attr_accessor :name, :height, :weight

        def initialize(n, h, w)
          @name = n
          @height = h
          @weight = w
        end

        def speak
          "#{name} says arf!"
        end

        def change_info(n, h, w)
          @name = n
          @height = h
          @weight = w
        end

        def info
          "#{name} weighs #{weight} and is #{height} tall."
        end
      end
      ```
    - Using the `change_info` method:
      ```ruby
      sparky = GoodDog.new('Sparky', '12 inches', '10 lbs')
      puts sparky.info      # => Sparky weighs 10 lbs and is 12 inches tall.

      sparky.change_info('Spartacus', '24 inches', '45 lbs')
      puts sparky.info      # => Spartacus weighs 45 lbs and is 24 inches tall.
      ```
    - Instead of accessing instance variables directly to change them, we want to use setter methods instead:
      ```ruby
      def change_info(n, h, w)
        name = n
        height = h
        weight = w
      end
      ```
    - But this doesn't work:
      ```ruby
      sparky.change_info('Spartacus', '24 inches', '45 lbs')
      puts sparky.info      # => Sparky weighs 10 lbs and is 12 inches tall.
      ```
    - What happened???
### Calling Methods with Self
- Ruby interpreted our `name = n` syntax as initializing a local variable, instead of as a call to the `name=` setter method that we intended.
- we need to use the syntax `self.name=` to make it clear to ruby that we are calling a method:
  ```ruby
  def change_info(n, h, w)
    self.name = n
    self.height = h
    self.weight = w
  end

  sparky.change_info('Spartacus', '24 inches', '45 lbs')
  puts sparky.info  # => Spartacus weighs 45 lbs and is 24 inches tall.
  ```
- This syntax can also be used for getter methods, but is not required:
  ```ruby
  def info
    "#{self.name} weighs #{self.weight} and is #{self.height} tall."
  end
  ```
- Prefixing `self` is not restricted to accessor methods (i.e. methods created by `attr_accessor`).
  - It can be used with any instance method.
  - For example, the `info` method is not created by `attr_accessor`:
    ```ruby
    class GoodDog
      # ... rest of code omitted for brevity
      def some_method
        self.info
      end
    end
    ```
  - This works, but the general rule from the Ruby Style Guide is: "Avoid `self` where not required."