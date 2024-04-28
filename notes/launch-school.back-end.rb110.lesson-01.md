---
id: 9taxto6jgd1fce2uh600s31
title: Lesson 01
desc: ''
updated: 1714340466987
created: 1714334082427
---
## Collections Basics
Collections are made up of individual elements.
To work with collections, must understand:
  - how they are structured
  - how to reference, assign, and mutate collections
  - how to reference, assign, and mutate individual elements within a collection

### Element Reference
#### Strings
- Integer based reference (starting at 0) for each character:

      
          my_string = 'abcde'

          my_string[2] # => 'c'
          my_string.slice(2) # => 'c'

          my_string[2, 3] # => 'cde'
          my_string.slice(2, 3) # => 'cde'

          my_string[1..3] # => 'bcd
          my_string.slice(1..3) # => 'bcd

          my_string['bcd'] # = 'bcd'
          my_string.slice('bcd') # = 'bcd'


- `String#slice` / `String#[]` method always returns a **new string**
    ```
    char1 = str[2]  # => "c"
    char2 = str[2]  # => "c"
    char1.object_id == char2.object_id  # => false
    ```

- Technically, strings are not true collections:
  - True collections contain multiple objects
  - Strings only contain one object
    - individual characters are not objects
      - they are just part of the object that contains the string value