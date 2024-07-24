---
id: xpv37uyksjagfbvtsqbkuoy
title: 03 Classes and Objects 2
desc: ''
updated: 1721831710238
created: 1721814781927
---
## Class Methods
- Instance methods pertain to instances / objects of a class
  - They can only be called on instantiated objects of the class
- **Class Methods** pertain to the class istself
  - They can be called directly on the class, without having to instantiate any objects
  - To **define** a class method, use the reserved: word `self`:
    ```ruby
    class GoodDog
      # ... rest of code ommitted for brevity

      def self.what_am_i  # Class method definition
        "I'm a GoodDog class!"
      end

      # ... rest of code ommitted for brevity
    end
    ```
  - To **invoke** a class method, use the class name followed by the method name:
    ```ruby
    GoodDog.what_am_i  # => I'm a GoodDog class!
    ```
  - Class methods allow the creatiion of functionality that does not pertain to individual objects
    - Objects contain state, so if a particular method does not need to deal with states, then it can be defined as a class method
## Class Variables
- **Instance variables** capture information related to specific instances of a class (i.e. objects)
- **Class variables** capture information related to an entire class
  - Syntax: `@@class_variable`
    ```ruby
    class GoodDog
      @@number_of_dogs = 0  # class variable

      def initialize  # instance method
        @@number_of_dogs += 1
      end

      def self.total_number_of_dogs  # class method
        @@number_of_dogs
      end
    end

    puts GoodDog.total_number_of_dogs  # => 0

    dog1 = GoodDog.new
    dog2 = GoodDog.new

    puts GoodDog.total_number_of_dogs  # => 2
    ```
  - We have a class variable named `@@number_of_dogs`
    - initialized to 0
  - The constructor (`initialize` method) increments `@@number_of_dogs` by 1 every time it is invoked (i.e. every time a new object of the class is created)
    - Class variables can be accessed from within an instance method
  - The class method `total_number_of_dogs` returns the value of the class variable `@@number_of_dogs`
  - In this example a class variable and a class method are used to keep track of a class level detail that pertains only to the class, not to individiual objects.
## Constants
- Used for variables that should not change over the course of the program's run.
  - Syntax:
    - Official: snake case but starting with an uppercase letter: `My_constant`
    - Conventional: snake case using all uppercase letters: `MY_CONSTANT`
  - Example:
    ```ruby
    class GoodDog
      DOG_YEARS = 7

      attr_accessor :name, :age

      def initialize(n, a)
        self.name = n
        self.age  = a * DOG_YEARS
      end
    end

    sparky = GoodDog.new("Sparky", 4)
    puts sparky.age  # => 28
    ```
## The `to_s` Method
- Built in to every class in Ruby
  ### Use by the `puts` Method:

  ```ruby
  puts sparky  # =>#<GoodDog:0x007fe542323320>
  ```
  - The `puts` method automatically calls `to_s` on its argument, which in this case is the object referenced by `sparky`.
    - By default, the `to_s` method returns the name of the object's class and an encoding of the object id.
    - Note: For an array, the `puts` method calls `to_s` on each element of the array (and writes them on separate lines).
  - The default `to_s` method that is built in to every class in Ruby can be overridden within a class definition:
    ```ruby
    class GoodDog
      DOG_YEARS = 7

      attr_accessor :name, :age

      def initialize(n, a)
        @name = n
        @age  = a * DOG_YEARS
      end

      def to_s
        "This dog's name is #{name} and it is #{age} in dog years."
      end
    end
    ```
  Result:
    ```ruby
    puts sparky  # => This dog's name is Sparky and is 28 in dog years.
    ```
  ### Use by the `p` Method
  - The `p` method is similar, but does not call `to_s` on its argument.
  - Instead it calls another built-in Ruby instance method called `inspect`
      ```ruby
      p sparky  # => #<GoodDog:0x007fe54229b358 @name="Sparky", @age=28>
      ```
  - `p sparky` is equivalent to `puts sparky.inspect`
  - It is best not to override the `inspect` method
  ### Use in String Interpolation
  - The `to_s` method is also automatically called in string interpolation:
    ```ruby
    irb :001 > arr = [1, 2, 3]
    => [1, 2, 3]
    irb :002 > x = 5
    => 5
    irb :003 > "The #{arr} array doesn't include #{x}."
    => "The [1, 2, 3] array doesn't include 5."
    ```
  - Because the `to_s` method was overridden in the `GoodDog` class definition, this affects string interpolation of any `GoodDog` objects:
    ```ruby
    irb :001 > "#{sparky}"
    => "This dog's name is Sparky and it is 28 in dog years."
    ```
  ### Overriding `#to_s`