---
id: xpv37uyksjagfbvtsqbkuoy
title: 03 Classes and Objects 2
desc: ''
updated: 1721986189498
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
  - The behavior of `#to_s` can be customized in a class.
  - When `#to_s` is invoked, Ruby looks up the inheritance chain for the nearest version of the method.
  - If it has not been customized in the class of the calling object, the nearest version is usually `Object#to_s`
  - Ruby expects `#to_s` to always return a string.
    - If it does not, the method won't work as expected in places where `#to_s` is implicitly invoked (like `#puts` and string interpolation).
    - If `#to_s` is defined to return something other than a string, Ruby will ignore the non-string value and look further up the inheritance chain for a version of `#to_s` that does return a string.
      - Usually it will use the value returned by `Object#to_s`.
    - Example: If `Foo#to_s` is defined to return the integer `42`, Ruby ignores that value and uses `Object#to_s` to supply the return value.
      ```ruby
      class Foo
        def to_s
          42
        end
      end

      foo = Foo.new
      puts foo      # Prints #<Foo:0x0000000100760ec0>
      puts "foo is #{foo}" # Prints: foo is #<Foo:0x0000000100760ec0>
      ```
    - If instead `Foo#to_s` is defined to return a string, the return value will be supplied by `Foo#to_s`:
      ```ruby
      class Foo
        def to_s
          "42"
        end
      end

      foo = Foo.new
      puts foo             # Prints 42
      puts "foo is #{foo}" # Prints: foo is 42
      ```
  - NOTE: if `#to_s` is overridden, it will only be invoked on objects of the class where the customized `#to_s` method is defined.
    ```ruby
    class Bar
      attr_reader :xyz
      def initialize
        @xyz = { a: 1, b: 2 }
      end

      def to_s
        'I am a Bar object!'
      end
    end

    bar = Bar.new
    puts bar      # Prints I am a Bar object!
    puts bar.xyz  # Prints {:a=>1, :b=>2}
    ```
  - In the above example, the `Bar` class has a customized `to_s` method.
  - `bar` references an object of the `Bar` class
    - So when `puts` is called, and `bar` is passed in as an argument, `puts` invokes the customized `Bar#to_s` method.
  - `bar.xyz`, however, does not return a `Bar` object
    - it returns the value referenced by instance variable `@xyz` (`{ a: 1, b: 2 }`)
    - so when `puts` is called, and `bar.xyz` is passed in as an argument, `puts` does **not** invoke the customized `Bar#to_s` method.
    - `puts` will use `Object#to_s` in this case
## More About `self`
- `self` can refer to different things depending on where it is used
  ### Setter Methods
  - `self` is used when calling setter methods from within the class.
    - this allows Ruby to correctly determine that an expression is invoking the setter method rather than initializing a local variable.
  ### Class Method Definitions
  - `self` acts as a reserved word and is prepended to method names when defining a class method.
  ### Examples
    #### Setter Methods
    - We define the class `GoodDog`:
      ```ruby
      class GoodDog
        attr_accessor :name, :height, :weight

        def initialize(n, h, w)
          self.name   = n
          self.height = h
          self.weight = w
        end

        def change_info(n, h, w)
          self.name   = n
          self.height = h
          self.weight = w
        end

        def info
          "#{self.name} weighs #{self.weight} and is #{self.height} tall."
        end
      end
      ```
    - We can see that `self` is used whenever we call an instance method from within the class.
    - What does `self` represent here? We will add a new instance method to help us find out:
      ```ruby
      class GoodDog
        # ... rest of code omitted for brevity

        def what_is_self
          self
        end
      end
      ```
    - We can now instantiate a new `GoodDog` object and invoke instance method `GoodDog#what_is_self` on it:
      ```ruby
      sparky = GoodDog.new('Sparky', '12 inches', '10 lbs')
      p sparky.what_is_self
      # => #<GoodDog:0x007f83ac062b38 @name="Sparky", @height="12 inches", @weight="10 lbs">
      ```
    - So from within a class, when an instance method uses `self`, it references the *calling object*.
      - In the above example, `self` references the `GoodDog` object assigned to `sparky`.
      - So from within the `change_info` method, calling `self.name=` acts the same as calling `sparky.name=` from *outside* the class.
        - `sparky.name` can't be called from within the class; `sparky` is out of scope.
    #### Class Method Definitions
      - When `self` is prepended to a mtehod definition, it defines a **class method**:
      ```ruby
      class MyAwesomeClass
        def self.this_is_a_class_method
        end
      end
      ```

