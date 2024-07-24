---
id: xpv37uyksjagfbvtsqbkuoy
title: 03 Classes and Objects 2
desc: ''
updated: 1721815527878
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

      def self.what_am_i         # Class method definition
        "I'm a GoodDog class!"
      end

      # ... rest of code ommitted for brevity
    end
    ```
  - To **invoke** a class method, use the class name followed by the method name:
    ```ruby
    GoodDog.what_am_i          # => I'm a GoodDog class!
    ```
