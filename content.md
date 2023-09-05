# Loops

## if: conditionally doing something once

Consider the following program, which utilizes an `if` statement. What do you expect the output of this program to be? Try to interpret the program yourself before you ask Ruby to:

```ruby
numbers = Array.new

if numbers.length < 10
  new_number = rand(100)
  numbers.push(new_number)
end

len = numbers.length

pp "The numbers array contains:"
pp numbers
pp "The length of it is: " + len.to_s
```
{: .repl #if_once title="if, doing it once" points="1"}

In the code, we need to start by **instantiating**, or "creating an instance of", the variable `numbers`. This is just an empty array, but we fill it in the `if` statement (by `.push`ing into it). 

- If we had *not* instantiated `numbers`, then what would have happened at the step `numbers.length`?
- The `if` statement would not evaluate and the code would just print the `numbers` and `len` variables
    - No, give it a try in the runnable code block.
- We would get an error message.
    - Yes! We love error messages: RTEM.
- We cannot call `.length` on an empty array
    - No, we can and we did.
{: .choose_best #instantiate title="Instantiate" points="1" answer="2" }

We start off with a blank array, `numbers`. If its length is less than `10` (this is true, since length is currently `0`), we push a new random number into it.

Once Ruby reaches the `if`'s `end`, it proceeds to the next line and continues to execute the rest of the code (whether the `if`'s condition was true or not).

At the end of the day, `numbers` has one element in it and `len` is `1`.

## while: conditionally doing something multiple times

Now, consider almost identical code, but with the `if` keyword swapped for a new keyword — `while`:

```ruby
numbers = Array.new

while numbers.length < 10
  new_number = rand(100)
  numbers.push(new_number)
end

len = numbers.length

pp "The numbers array contains:"
pp numbers
pp "The length of it is: " + len.to_s
```
{: .repl #while_loop title="while, looping" points="1"}

`while` works almost exactly like `if` — it evaluates the expression next to it, and if the expression is truthy, it executes the code on the lines between it and its `end`; if not, it ignores the code on the lines between it and its `end`.

There is one key difference: if the condition next to a `while` is truthy, after we reach its `end`, the execution of the program **jumps back up to the `while` statement**.

Then the condition is evaluated *again*. **If it is *still* true, then the code inside the `while` statement is executed _again_.** And then the execution of the program jumps back up to the `while` statement *again*. Etc.

So in this case,

 - The first time we reach the `end`, we jump back up to the `while`.
 - Evaluate `a.length < 10` again — still true, since `1 < 10`.
 - So we push in another random number, and jump back up.
 - `2 < 10`? Yep, so we do it again.
 - We push in another random number, and jump back up.
 - `3 < 10`? Yep, so we do it again.
 - `4 < 10`? Yep, so we do it again.
 - Etc.
 - `10 < 10`? Nope, so now proceed to the line after the `end` and continue.
 - And `len` ends up being `10`.

What we've seen here is our very first **loop**; code that is executed multiple times. It could be an arbitrary number of times, perhaps even an infinite number of times if we aren't careful.

## Blocks

Fundamentally, all looping is implemented with `while`; but, this being Ruby, there are all sorts of convenience methods on top to make it as easy as possible to create loops for various contexts. For example, let's say I wanted to print:

```
"1 Mississippi"
"2 Mississippi"
"3 Mississippi"
# etc
"10 Mississippi"
```

exactly 10 times. I could do it using `while` like this; try interpreting the following code before you click "Run":

```ruby
mississippis = 1

while mississippis <= 10
  pp mississippis.to_s + " Mississippi"

  mississippis = mississippis + 1
end
```
{: .repl #mississippi_while title="while: Mississippis" points="1"}

Does the code make sense to you?

If the line `mississippis = mississippis + 1` looks a little odd to you, you're not alone. Remember, this is _variable assignment_ with the `=`, not equivalence with `==`. So the expression on the right side (`mississippis + 1`) is evaluated _first_ until there's just one object (e.g. `2`) left; and then that object replaces the contents of the variable (`mississippis`) named on the left. Rinse and repeat.

Or, rather than `while`, I could use `Integer`'s `.times` method, like this:

```ruby
mississippis = 1

10.times do
  pp mississippis.to_s + " Mississippi"

  mississippis = mississippis + 1
end
```
{: .repl #mississippi_times title="times: Mississippis" points="1"}

Notice there's a new keyword here: `do`. This is because the `.times` method, in order to do its job of executing some code 10 times, needs a special argument — _the code to execute_.

In order to pass a method _some lines of code_ as an argument, we need to wrap the lines of code within the `do` and `end` keywords, creating what's called a **block** of code.

So, given a **block** of code, the `10.times` method will execute it for us exactly 10 times; this saves us the trouble of writing a condition for `while`.

### Practice looping with blocks

Practice is crucial! Below are some graded code blocks to give you practice writing programs with `do`-`end` looping blocks.

#### Letter count

In the graded code block below, write a program that takes a randomly `sample`d `word` and counts from 1 to however long the word is, then prints the word and its length as a final output. For example, if the `word` was `apple`, the output should be:

```
1
2
3
4
5
"apple is 5 letters long!"
```

Use what you learned about looping and blocks above to get the tests to pass!

Note: be sure to use `pp` to print the output (do not use `p` or `puts`; alternative printing methods).

```ruby
word = ["apple", "fantasmagorical", "a"].sample
# write your program here
```
{: .repl #letter_count title="Letter count" readonly_lines="[1]"}

```ruby
describe "Letter count" do
  it "should print '1 2 3 4 5 6 'banana is 6 letters long!'' if the input is 'banana'" do
    path = "/tmp/code.rb"

    # specify a word
    file = File.read(path)
    new_content = file.split("\n")
    new_content = new_content.map { |line| line == 'word = ["apple", "fantasmagorical", "a"].sample' ? 'word = "banana"' : line }.join("\n")
    File.open(path, 'w') { |line| line.puts new_content }

    expect { require_relative(path) }.to output("1\n2\n3\n4\n5\n6\n\"banana is 6 letters long!\"\n").to_stdout
  end
end
```
{: .repl-test #letter_count_test_1 for="letter_count" title="Letter count should print '1 2 3 4 5 6 'banana is 6 letters long!'' if the input is 'banana'" points="1"}

```ruby
describe "Letter count" do
  it "should print '1 2 3 'for is 3 letters long!'' if the input is 'for'" do
    path = "/tmp/code.rb"

    # specify a word
    file = File.read(path)
    new_content = file.split("\n")
    new_content = new_content.map { |line| line == 'word = ["apple", "fantasmagorical", "a"].sample' ? 'word = "for"' : line }.join("\n")
    File.open(path, 'w') { |line| line.puts new_content }

    expect { require_relative(path) }.to output("1\n2\n3\n\"for is 3 letters long!\"\n").to_stdout
  end
end
```
{: .repl-test #letter_count_test_2 for="letter_count" title="Letter count should print '1 2 3 'for is 3 letters long!'' if the input is 'for'" points="1"}

#### FizzBuzz

Got it? Great! Let's get some more practice though, since loops (and **conditionals**, which you'll need here) are _so_ important, e.g.:

```ruby
10.times do
  if # some condition
    # do this thing
  else
    # do this other thing
  end
end
```

In the next graded code block, write a program that prints the numbers from 1 to 30, except:
  - for multiples of three print "Fizz" instead of the number
  - for multiples of five print "Buzz" instead of the number
  - for numbers which are multiples of both three and five print "FizzBuzz"

Your output should look like:

```
1
2
"Fizz"
4
"Buzz"
# etc
29
"FizzBuzz"
```

Hint: if x is a multiple of y, that means that we can divide x by y and have nothing leftover. Do we have anything in our Ruby toolbox that can help find _remainders_? Look through the previous Ruby lessons, you have all the tools you need to solve this with `Integer` math, `if` statement conditionals, and loops.

Note: be sure to use `pp` to print the output (do not use `p` or `puts`; alternative printing methods).

```ruby
# write your program here
```
{: .repl #fizzbuzz title="FizzBuzz"}

```ruby
describe "FizzBuzz" do
  it "should print the correct output" do
    path = "/tmp/code.rb"
    expect { require_relative(path) }.to output("1\n2\n\"Fizz\"\n4\n\"Buzz\"\n\"Fizz\"\n7\n8\n\"Fizz\"\n\"Buzz\"\n11\n\"Fizz\"\n13\n14\n\"FizzBuzz\"\n16\n17\n\"Fizz\"\n19\n\"Buzz\"\n\"Fizz\"\n22\n23\n\"Fizz\"\n\"Buzz\"\n26\n\"Fizz\"\n28\n29\n\"FizzBuzz\"\n").to_stdout
  end
end
```
{: .repl-test #fizzbuzz_test_1 for="fizzbuzz" title="FizzBuzz should print the correct output" points="1"}

### Block variables 

Great work on those programming exercises, hopefully they were fun puzzles to think through. Now, let's discuss another aspect of `do`-`end` blocks.

The `.times` method will save us even more trouble; we can stop worrying about creating and incrementing the counter variable, `mississippis` from our previous examples. The `.times` method will create a **block variable** and assign values to it for us automatically, but we have to choose a name for it using some new syntax after the `do`: the vertical bars, `| |`, or "pipes". It looks like this:

```ruby
10.times do |mississippis|
  pp mississippis.to_s + " Mississippi"
end
```
{: .repl #mississippi_block_var title="block variable: Mississippis" points="1"}

Try running it. Here's what's going on:

 - We created a block of code with `do`/`end` and gave it to `.times`.
 - We chose a name for a **block variable**, `mississippis`, with the `| |` after the `do`.
 - Behind the scenes, the `.times` method did `mississippis = 0` before the first iteration.
 - The `.times` method executed the block of code the first time.
 - Behind the scenes, the `.times` method did `mississippis = 1` before the second iteration.
 - The `.times` method executed the block of code the second time.
 - Etc.

Why does `.times` start by assigning `0` to its block variable during the first iteration, rather than `1`? Well, that's just how the author of the `.times` method made it work. Remember, Ruby, like many other languages uses [zero-indexing](https://learn.firstdraft.com/lessons/73#at).

Fortunately, Ruby provides lots of other looping convenience methods that we can take advantage of instead, and each one assigns different values to its block variable.

In the runnable code block above, replace `10.times` with each of the following and play around with the arguments to get a sense of how each method works:

```ruby
5.upto(10)
99.downto(90)
1.step(10, 3)
10.step(1, -4)
```

- Select all that apply:
- In `2.upto(10)`, the count begins with 3 and goes to 9
    - Really? Did you try it?
- In `0.step(10, 2)`, `0` is the start, `10` (first argument) is the step, and `2` (second argument) is the highest value
    - Really? Did you try it?
- In `1.step(10, 2)`, `1` is the start, `10` (first argument) is the highest value, and `2` (second argument) is the step
    - Yes!
- In `2.step(50, 10)`, the count would proceed like `2`, `10`, `20`, etc.
    - Really? Did you try it?
- In `1.upto(11)`, the count would proceed like `1`, `2`,...,`11`
    - Yes!
{: .choose_all #ranges title="Alternatives to times" points="2" answer="[3,5]" }

Let's get a bit more practice with loops and block variables before we leave this lesson.

In the graded code block below, write a program that, given one of the randomly `sample`d `n`umbers, prints a multiplication table of the number from 1 to 10.

For example, given `n = 2`, the output should be:

```
2
4
6
8
10
12
14
16
18
20
```

Get the tests to pass, and you are done with this lesson!

Note: be sure to use `pp` to print the output (do not use `p` or `puts`; alternative printing methods).

```ruby
n = [3, 10, 15].sample
# write your program here
```
{: .repl #multiples title="Multiples" readonly_lines="[1]"}

```ruby
describe "Multiples" do
  it "should print '0 0 0 0 0 0 0 0 0 0' if the input is '0'" do
    path = "/tmp/code.rb"

    # specify a word
    file = File.read(path)
    new_content = file.split("\n")
    new_content = new_content.map { |line| line == 'n = [3, 10, 15].sample' ? 'n = 0' : line }.join("\n")
    File.open(path, 'w') { |line| line.puts new_content }

    expect { require_relative(path) }.to output("0\n0\n0\n0\n0\n0\n0\n0\n0\n0\n").to_stdout
  end
end
```
{: .repl-test #multiples_test_1 for="multiples" title="Multiples should print '0 0 0 0 0 0 0 0 0 0' if the input is '0'" points="1"}

```ruby
describe "Multiples" do
  it "should print '4 8 12 16 20 24 28 32 36 40' if the input is '4'" do
    path = "/tmp/code.rb"

    # specify a word
    file = File.read(path)
    new_content = file.split("\n")
    new_content = new_content.map { |line| line == 'n = [3, 10, 15].sample' ? 'n = 4' : line }.join("\n")
    File.open(path, 'w') { |line| line.puts new_content }

    expect { require_relative(path) }.to output("4\n8\n12\n16\n20\n24\n28\n32\n36\n40\n").to_stdout
  end
end
```
{: .repl-test #multiples_test_2 for="multiples" title="Multiples should print '4 8 12 16 20 24 28 32 36 40' if the input is '4'" points="1"}

##  Conclusion

Now that we have an understanding of loops, we can have a look at the super important `.each` method for iterating over data structures.

- Approximately how long (in minutes) did this lesson take you to complete?
{: .free_text_number #time_taken title="Time taken" points="1" answer="any" }

<span style="font-size: large">**When you are done here, close the window and return to Canvas for the next lesson in the series.**</span>

----
