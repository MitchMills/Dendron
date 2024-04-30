---
id: wzttpb1tyeuyqd4vnecsy3v
title: Looping
desc: ''
updated: 1714511401030
created: 1714487288444
---
## Looping
- Can be used to perform an action or actions on each element in a collection.
  ```ruby
  numbers = [1, 2, 3, 4, 5]
  counter = 0

  loop do
    numbers[counter] += 1
    counter += 1
    break if counter == numbers.size
  end

  numbers # => [2, 3, 4, 5, 6]
  ```
### Basic Loop Elements
  - a loop (`loop`, `for`, `while`, `until`)
    - NB: `loop` is a method (`Kernel#loop`), but `for`, `while` and `until` are control expressions (this affects scoping rules)
  - a counter / way to control number of iterations
  - a way to retrieve current value
  - a way to exit loop
### Controlling a Loop
- `Kernel#loop` method with a block
- Can use reserved word `break` to exit loop
- Can use conditional statements to control when `break` is executed
  
### Iteration
- Can use a counter to control the number of iterations
  ```ruby
  counter = 0

  loop do
    puts 'Hello!'
    counter += 1
    break if counter == 5
  end
  ```
- Counter must be initialized outside the loop. Otherwise it will be reassigned to its initial value on each iteration and the `break` condition will never be met
#### Break Placement
- `break` on last line:
  - mimics behavior of a `do/while` loop
  - code within block is guaranteed to execute at least once
- `break` on first line:
  - mimics behavior of a `while` loop
  - code within block may or may not execute, depending on the condition
#### `next` Keyword
- `next` skips the rest of the current iteration and begins the next one
  ```ruby
  counter = 0

  loop do
    counter += 1
    next if counter.odd?
    puts counter
    break if counter > 5
  end

  # => 2, 4, 6
  ```
- Note: must increment *before* `next` so that incrementing isn't skipped
- Note: if `break` conditional was `counter == 5`, it would never execute, because when the counter is odd that iteration is skipped
  
### Iterating Over Collection
#### Iterating Over Strings
- Example
  ```ruby
  alphabet = 'abcdefghijklmnopqrstuvwxyz'
  counter = 0

  loop do
    break if counter >= alphabet.size
    puts alphabet[counter]
    counter += 1
  end
  ```
#### Iterating Over Arrays
- Example
  ```ruby
  objects = ['hello', :key, 10, []]
  counter = 0

  loop do
    break if counter >= objects.size
    puts objects[counter].class
    counter += 1
  end
  ```
  #### Iterating Over Hashes
  - A bit more complicated due to use of key-value pairs instead of indexes. Therefore can't use a simple counter to control interation.
    ```ruby
    number_of_pets = {
      'dogs' => 2,
      'cats' => 4,
      'fish' => 1
    }

    pets = number_of_pets.keys # => ['dogs', 'cats', 'fish']
    counter = 0

    loop do
      break if counter == number_of_pets.size
      current_pet = pets[counter]
      current_pet_number = number_of_pets[current_pet]
      puts "I have #{current_pet_number} #{current_pet}!"
      counter += 1
    end
    ```
