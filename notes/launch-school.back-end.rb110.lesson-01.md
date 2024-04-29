---
id: 9taxto6jgd1fce2uh600s31
title: Lesson 01
desc: ''
updated: 1714407608470
created: 1714334082427
---
## Collections Basics
Collections are made up of individual elements.
To work with collections, must understand:
  - how they are structured
  - how to reference, assign, and mutate collections
  - how to reference, assign, and mutate individual elements within a collection

### Element Reference
#### String Element Reference
- Integer based reference (starting at 0) for each character:

  ```
  my_string = 'abcde'

  my_string[2] # => 'c'
  my_string.slice(2) # => 'c'

  my_string[2, 3] # => 'cde'
  my_string.slice(2, 3) # => 'cde'

  my_string[1..3] # => 'bcd'
  my_string.slice(1..3) # => 'bcd'

  my_string['bcd'] # = 'bcd'
  my_string.slice('bcd') # = 'bcd'
  ```
  

- `String#slice` / `String#[]` method always returns a **new string**
    ```
    char1 = str[2]  # => "c"
    char2 = str[2]  # => "c"
    char1.object_id == char2.object_id  # => false
    ```

- Technically, strings are not true collections:
  - True collections contain multiple objects
  - Strings only contain one object, the string value
    - Individual characters of a string are not objects
    - Individual characters are just part of the object that contains the string value
  - Strings behave like collections in many ways, and many string methods work similarly to (and often share names with) methods for true collections
   
#### Array Element Reference
- Arrays are lists of elements ordered by index
- Elements can be any ruby object
- Integer-based reference (starting at 0) for each element
  ```
  my_array = [1, 2, 3, 4, 5]

  my_array[2] # => 3
  my_array.slice(2) # => 3

  my_array[2, 3] # => [3, 4, 5]
  my_array.slice(2, 3) # => [3, 4, 5]

  my_array[1..3] # => [2, 3, 4]
  my_array.slice(1..3) # => [2, 3, 4]
  ```   
   

- `Array#slice` / `Array#[]` method always returns either a new array **or** a single element.
  ```
  this_array = ['a', 'b', 'c', 'd', 'e']

  this_array[3, 1] # => ['d']  
  this_array[3..3] # => ['d']  
  this_array[3] # => 'd'
  ```
     

#### Hash Element Reference
- Composed of key-value pairs
- Keys and values can be any ruby object
- Values are referenced using keys
- Keys must be **unique**
  - Values with identical keys get overwritten
- Values need not be unique
  ```
  my_hash = { :fruit => 'apple', :vegetable => 'carrot' }

  my_hash['fruit'] # => 'apple'
  my_hash.keys # => [:fruit, :vegetable]
  my_hash.vaules # => ['apple', 'carrot']
  ```

### Element Reference Gotchas
#### Out of Bounds Indices
- Referencing an out-of-bounds index in a string or array returns `nil`
  - Strings never contain `nil` (they can contain `'nil'`), so a return of `nil` indicates an out-of-bounds index was used
  - Arrays CAN contain `nil` though
    ```
    an_array = [3, 'd', nil]

    an_array[2] # => nil
    an_array[7] # => nil
    ```
- How to tell the difference between a valid return and an out-of-bounds reference?
    - `Array#fetch`: "Tries to return the element at position index, but throws an IndexError exception if the referenced index lies outside of the array bounds."
      ```
      an_array.fetch(2) # => nil
      an_array.fetch(7) # => IndexError: index 7 outside of array bounds: -3...3
      an_array.fetch(7, "Uh oh") # => "Uh oh"
      ```
#### Negative Indices
- Negative indices start from the last index in the string or array and work backwards
  ```
  my_string = 'abcde'
  my_string[-2] # => 'd'
  my_string[-6] # => nil
  my_string.fetch(-6) # => IndexError . . . 

  my_array = [1, 2, 3, 4, 5]
  my_array[-2] # => 4
  my_array[-7] # => nil
  my_array.fetch(-7) # => IndexError . . .
  ```
#### Invalid Hash Keys
- Using a non-existant key returns `nil`
- BUT, `nil` can also be a valid hash value
  ```
  cool_hash = { :a => 1, 'b' => 'two', :c => nil }

  cool_hash['b'] # => 'two'
  cool_hash[:c] # => nil # valid hash value
  cool_hash['c'] # => nil # non-existant key
  ```
- `Hash#fetch` wil throw a `KeyError` if a non-existant key is used.
  ```
  cool_hash.fetch(:c) # => nil
  cool_hash.fetch('c') # => KeyError: key not found: "c"
  
### Conversion
#### Strings and Arrays
- Because strings and arrays share many similarities, such as both being zero-indexed collections, they can be converted into each other fairly easily.
  ```
  my_string = 'Practice'
  my_string.chars # => ["P", "r", "a", "c", "t", "i", "c", "e"]

  my_array = ["P", "r", "a", "c", "t", "i", "c", "e"]
  my_array.join # => 'Practice'

  question = 'How do you get to Carnegie Hall?'
  words = question.split # => ["How", "do", "you", "get", "to", "Carnegie", "Hall?"]
  words.join # => "HowdoyougettoCarnegieHall?"
  words.join(' ') # => 'How do you get to Carnegie Hall?'
  ```
#### Hashes and Arrays
- Hashes can be converted into nested arrays:
  ```
  item_colors = { sky: 'blue', grass: 'green' }
  item_colors.to_a # => [[:sky, 'blue'], [:grass, 'green]]
  ```
- Arrays can be converted into hashes:
  ```
  details = [[:name, 'Joe'], [:age, 10], [:favorite_color, 'blue']]
  details.to_h # => { name: 'Joe', age: 10, favorite_color: 'blue' }
  ```
    
  ### Element Assignment
  