---
id: wzttpb1tyeuyqd4vnecsy3v
title: Looping
desc: ''
updated: 1714488546587
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
    break if counter == alphabet.size
    puts alphabet[counter]
    counter += 1
  end
  ```
  
