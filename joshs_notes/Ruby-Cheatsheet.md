[back to overwiev](/../..)  
Looking for [Rails](../master/Ruby-on-Rails-Cheatsheet.md)?

# Ruby Cheatsheet

##### Table of Contents

- [Basics](#basics)  
- [Vars, Constants, Arrays, Hashes & Symbols](#vars-constants-arrays-hashes--symbols)  
- [Methods](#methods)
- [Classes](#classes)
- [Modules](#modules)
- [Blocks & Procs](#blocks--procs)  
- [Lambdas](#lambdas)
- [Calculation](#calculation)  
- [Comment](#comment)  
- [Conditions](#conditions)  
- [Printing & Putting](#printing--putting)  
- [User Input](#user-input)  
- [Loops](#loops)
- [Sorting & Comparing](#sorting--comparing)  
- [Usefull Methods](#usefull-methods)

## Basics

- `$ irb`: to write ruby in the terminal
- don't use `'` in ruby, use `"` instead
- you can replace most `{}` with `do end` and vice versa –– not true for hashes or `#{}` escapings
- Best Practice: end names that produce booleans with question mark
- CRUD: create, read, update, delete
- `[1,2].map(&:to_i)`
- `integer`: number without decimal
- `float`: number with decimal
- tag your variables:
- - `$`: global variable
- - `@`: instance variable
- - `@@`: class variable
- `1_000_000`: 1000000 –– just easier to read\*

## Vars, Contants, Arrays, Hashes & Symbols

```Ruby 
my_variable = “Hello”
my_variable.capitalize! # ! changes the value of the var same as my_name = my_name.capitalize
my_variable ||= "Hi" # ||= is a conditional assignment only set the variable if it was not set before.
```

### Constants

```Ruby
MY_CONSTANT = # something
```

### Arrays

```Ruby
my_array = [a,b,c,d,e]
my_array[1] # b
my_array[2..-1] # c , d , e
multi_d = [[0,1],[0,1]]
[1, 2, 3] << 4 # [1, 2, 3, 4] same as [1, 2, 3].push(4)
```

### Hashes

```Ruby
hash = { "key1" => "value1", "key2" => "value2" } # same as objects in JavaScript
hash = { key1: "value1", key2: "value2" } # the same hash using symbols instead of strings
my_hash = Hash.new # same as my_hash = {} – set a new key like so: pets["Stevie"] = "cat"
pets["key1"] # value1
pets["Stevie"] # cat
my_hash = Hash.new("default value")
hash.select{ |key, value| value > 3 } # selects all keys in hash that have a value greater than 3
hash.each_key { |k| print k, " " } # ==> key1 key2
hash.each_value { |v| print v } # ==> value1value2

my_hash.each_value { |v| print v, " " }
# ==> 1 2 3
```

### Symbols

```Ruby
:symbol # symbol is like an ID in html. :Symbols != "Symbols"
# Symbols are often used as Hash keys or referencing method names.
# They can not be changed once created. They save memory (only one copy at a given time). Faster.
:test.to_s # converts to "test"
"test".to_sym # converts to :test
"test".intern # :test
# Symbols can be used like this as well:
my_hash = { key: "value", key2: "value" } # is equal to { :key => "value", :key2 => "value" }
```

#### Functions to create Arrays

```Ruby
"bla,bla".split(“,”) # takes sting and returns an array (here  ["bla","bla"])
```

## Methods

**Methods**

```Ruby
def greeting(hello, *names) # *name is a splat argument, takes several parameters passed in an array
  return "#{hello}, #{names}"
end

start = greeting("Hi", "Justin", "Maria", "Herbert") # call a method by name

def name(variable=default)
  ### The last line in here get's returned by default
end
```

## Classes

_custom objects_

```Ruby
class ClassName # class names are rather written in PascalCase (It is similar to camelcase, but the first letter is capitalized)
  @@count = 0
  attr_reader :name # make it readable
  attr_writer :name # make it writable
  attr_accessor :name # makes it readable and writable

  def Methodname(parameter)
    @classVariable = parameter
    @@count += 1
  end

  def self.show_classVariable
    @classVariable
  end

  def Person.get_counts # is a class method
    return @@count
  end

  private

  def private_method; end # Private methods go here
end

matz = Person.new("Yukihiro")
matz.show_name # Yukihiro
```

### inheritance

```Ruby
class DerivedClass < BaseClass; end # if you want to end a Ruby statement without going to a new line, you can just type a semicolon.

class DerivedClass < Base
  def some_method
    super(optional args) # When you call super from inside a method, that tells Ruby to look in the superclass of the current class and find a method with the same name as the one from which super is called. If it finds it, Ruby will use the superclass' version of the method.
      # Some stuff
    end
  end
end

# Any given Ruby class can have only one superclass. Use mixins if you want to incorporate data or behavior from several classes into a single class.
```

## Modules

```Ruby
module ModuleName # module names are rather written in PascalCase
  # variables in modules doesn't make much sence since modules do not change. Use constants.
end

Math::PI # using PI constant from Math module. Double colon = scope resolution operator = tells Ruby where you're looking for a specific bit of code.

require 'date' # to use external modules.
puts Date.today # 2016-03-18

module Action
  def jump
    @distance = rand(4) + 2
    puts "I jumped forward #{@distance} feet!"
  end
end

class Rabbit
  include Action # Any class that includes a certain module can use those module's methods! This now is called a Mixin.
  extend Action # extend keyword mixes a module's methods at the class level. This means that class itself can use the methods, as opposed to instances of the class.
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

peter = Rabbit.new("Peter")
peter.jump # include
Rabbit.jump # extend
```

## Blocks & Procs

### Code Blocks

_Blocks are not objects_ A block is just a bit of code between do..end or {}. It's not an object on its own, but it can be passed to methods like .each or .select.

```Ruby
def yield_name(name)
  yield("Kim") # print "My name is Kim. "
  yield(name) # print "My name is Eric. "
end

yield_name("Eric") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric.
yield_name("Peter") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric. My name is Kim. My name is Peter.
```

### Proc

_saves blocks and are objects_ A proc is a saved block we can use over and over.

```Ruby
cube = Proc.new { |x| x ** 3 }
[1, 2, 3].collect!(&cube) # [1, 8, 27] # the & is needed to transform the Proc to a block.
cube.call # call procs directly
```

## Lambdas

```Ruby
lambda { |param| block }
multiply = lambda { |x| x * 3 }
y = [1, 2].collect(&multiply) # 3 , 6
```

Diff between blocs and lambdas:

- a lambda checks the number of arguments passed to it, while a proc does not (This means that a lambda will throw an error if you pass it the wrong number of arguments, whereas a proc will ignore unexpected arguments and assign nil to any that are missing.)
- when a lambda returns, it passes control back to the calling method; when a proc returns, it does so immediately, without going back to the calling method.
  So: A lambda is just like a proc, only it cares about the number of arguments it gets and it returns to its calling method rather than returning immediately.

## Calculation

- Addition (+)
- Subtraction (-)
- Multiplication (\*)
- Division (/)
- Exponentiation (\*\*)
- Modulo (%)
- The concatenation operator (<<)
- you can do 1 += 1 –– which gives you 2 but 1++ and 1-- does not exist in ruby
- `"A " << "B"` => `"A B"` but `"A " + "B"` would work as well but `"A " + 4 + " B"` not. So rather use string interpolation (`#{4}`)
- `"A #{4} B"` => `"A 4 B"`

## Comment

```Ruby
=begin
Bla
Multyline comment
=end
```

```Ruby
# single line comment
```

## Conditions

### IF

```Ruby
if 1 < 2
puts “one smaller than two”
elsif 1 > 2 # *careful not to mistake with else if. In ruby you write elsif*
puts “elsif”
else
puts “false”
end
# or
puts "be printed" if true
puts 3 > 4 ? "if true" : "else" # else will be putted
```

### unless

```Ruby
unless false # unless checks if the statement is false (opposite to if).
puts “I’m here”
else
puts “not here”
end
# or
puts "not printed" unless true
```

### case

```Ruby
case my_var
  when "some value"
    ###
  when "some other value"
    ###
  else
    ###
end
# or
case my_var
  when "some value" then ###
  when "some other value" then ###
  else ###
end
```

- `&&`: and
- `||`: or
- `!`: not
- `(x && (y || w)) && z`: use parenthesis to combine arguments
- problem = false
- print "Good to go!" unless problem –– prints out because problem != true

## Printing & Putting

```Ruby
print "bla"
puts "test" # puts the text in a separate line
```

## String Methods

```Ruby
"Hello".length # 5
"Hello".reverse # “olleH”
"Hello".upcase # “HELLO”
"Hello".downcase # “hello”
"hello".capitalize # “Hello”
"Hello".include? "i" # equals to false because there is no i in Hello
"Hello".gsub!(/e/, "o") # Hollo
"1".to_i # transform string to integer –– 1
"test".to_sym # converts to :test
"test".intern # :test
:test.to_s # converts to "test"
```

## User Input

```Ruby
gets # is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp # removes extra line created after gets (usually used like this)
```

## Loops

### While loop:

```Ruby
i = 1
while i < 11
  puts i
  i = i + 1
end
```

### Until loop:

```Ruby
i = 0
until i == 6
  puts i
  i += 1
end
```

### For loop

```Ruby
for i in 1...10 # ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)
  puts i
end
```

### Loop iterator

```Ruby
i = 0
loop do
  i += 1
  print "I'm currently number #{i}” # a way to have ruby code in a string
  break if i > 5
end
```

### Next

```Ruby
for i in 1..5
  next if i % 2 == 0 # If the remainder of i / 2 is zero, we go to the next iteration of the loop.
  print i
end
```

### .each

```Ruby
things.each do |item| # for each things in things do something while storing that things in the variable item
  print “#{item}"
end
```

on hashes like so:

```Ruby
hashes.each do |x,y|
  print "#{x}: #{y}"
end
```

### .times

```Ruby
10.times do
  print “this text will appear 10 times”
end
```

### .upto / .downto

```Ruby
10.upto(15) { |x| print x, " " } # 10 11 12 13 14 15
"a".upto("c") { |x| print x, " " } # a b c
```

## Sorting & Comparing

```Ruby
array = [5,4,1,3,2]
array.sort! # = [1,2,3,4,5] – works with text and other as well.
"b" <=> "a" # = 1 – combined comparison operator. Returns 0 if first = second, 1 if first > second, -1 if first < second
array.sort! { |a, b| b <=> a } # to sort from Z to A instead of A to Z
```

## Usefull Methods

```Ruby
1.is_a? Integer # returns true
:1.is_a? Symbol # returns true
"1".is_a? String # returns true
[1,2,3].collect!() # does something to every element (overwrites original with ! mark)
.map() # is the same as .collect
1.2.floor # 1 # rounds a float (a number with a decimal) down to the nearest integer.
cube.call # implying that cube is a proc, call calls procs directly
Time.now # displays the actual time
```


# Extra from class

`word_array = %w{ some bunchof words}` will split it for me into an array of words. can also use `(`, `)` to demarcate the start and end too. 

## Ruby string slicing
`array [1..3]` will slice an array. Also allows negetive refereces eg -1 to get the last element. 


## better way to print
use `p` rather than puts or print. Its is like print but with a new line. 

## maps
``` ruby 
array.map { |b| b.capitalize } 
``` 
will capitalize each element in the array

## Select/Reject
``` ruby 
array.select { |b| <some bool test> } 
``` 
will let you select some sub array based on the elements that pass the test. 
Reject will do the opposite. 
## Predicate methods

methods ending with '?' are called predicate methods, and only return true or false (by convention)

## `delete_if`
will remove all elements from an array that pass the bool test

## Hashes
use `=>` called a hash rocket instead of `:`. ruby does not have dot notation for hash maps. 

merge two hashes:
`h.merge <other hash>`

can leave a trailing comma after the last element in a hash. 

## Symbols for keys

for hashs, using strings as keys use memory every time. For each occurance of the key. it is better to use `:symbols` to avoid the extra memory overhead. Only store unique ID keys as strings (or things that need spaces, and wont occur too much).

```ruby
"some unique id" => {:ids_data_in_sub_hash => [data]}
```

##  symbol to proc
`.select{|fn| fn.even?}` can be written `.select &:even? ` but that is beyond what we need to know at the moment. 

## `.inject`
another advanced feature to be aware of.
```ruby 
def hamming_distance good, bad
  differences = correct.split('').map.with_index {|x, i| x != bad[i]} # gets a true/false array of differnces between two arrays
  puts differences.map {|x| x ? something_if_x_is_true : something_if_x_is_false}

```


## Range
`n...m` excludes the last number of a range; `n..m` is inclusive

## recursion
store sums/memory for recursion using extra parameters. 

```ruby
def recursive_function value, sum =default_value
  # exit condition
return(recursive_function (value-1, sum))

```

# Classes in Ruby - like JS factories. 

```ruby
class MyClass
end
``` 
creates a class


classes are named using `SnakeCase`. This is the format for **Constants** in ruby
## setter

a setter is a method that sets a value

## getter

a getter is the same as a setter, but it only gets a value, without needing an input. There is normally a setter and a getter both with the same name. 

```ruby
class = MyClass
  #setter 
  def property=(value) # this is a method called property=
    @property = value
  end
  #getter 
  def property
    @property
  end
end
```
`grouch.instrument=` === `grouch.intrument =`. This is syntactic sugar, letting you do something that isnt quite true. 
Both of these expressions are passing a value to the `instrument= ` function, its not setting it right here explicitly; as there may be further processing.  

in another language the equals would be part of a variable name, but its weird in ruby. 

Think of it as you are allowed an equals char in a method name, rather than you are allowed an equals as an assignment in the `def` like. ie `def method = (value)` is not true, but `method_name = method=; def method_name (value)`. 



## Ruby Macro

`attr_accessor: :name, :instrument, :vice, :etc` will write a getter and setter for you. 
You only want to use this when you dont need custom getters; ie you just need to raw input. 

## magic method

if you dont create an initialize clas in a ruby clas, then ruby will create that method for you.
`initialize` will run automatically. Can't call it init or have any other typos. 

initialize will let pass arguments to the `.new` method when creating new objects. 

```ruby
def initialize (i='', v='alcohol')
  @instruiment = i
  @vice = v
end
```
then you can use 
`brother.new "guitar", "cigars"` to create and initialise a new object, even with default values. 

## Class Inheritance. 

AKA parent class, ancestor, supercass and subclasses. 

```ruby 
class cat > animal # inherits the animal class, as well as being able to define its own classes.
  #...
end
```
Normally we will not be creating the superclass, just the subclass. ie creating a database or website. 

If the child needs its own init, then you need to call the parents init as well, or define everything in the parents init, in your subclass' init also. -> best to just call the parents inside your own init. 

Keep aware that you may often need to refactor methods out of your classe into parent classes. Learn to love parent classes. 

# Ruby On Rails
installation:
```ruby
gem uninstall rails
gem install rails -v 5.2.4.3
```
##  Active record

This is an **ORM** (object relational mapper)

Rails can convert our code which uses SQL to one that just uses rails, which has the added benifits of ease of use, input sanitisation and others. 
This is done using **active record**

SQL equivalents: 
1. `.new` (CREATE) creates a new object to save to the database.

1. `.save` (UPDATE) on an object will save objects to a database, and will do all the necessary input sanitation. 

1. `.view` (READ) Gets individual items

1. `.find` with `.save` can be used to update info. find the object, set the values, save it. val = `db.find()` then save. 

## create new project

* Gemfile - where you install gems

* **routes** is like our older main.rb. This is where the the routes are all established. [more info](http://guides.rubyonrails.org/routing.html)

### Controllers
```ruby
Rails.application.routes.draw do
  # route         controller#action
  get 'hello' => 'pages#hello'
end
```

run `rails server` from the dir of the gemfile to start your application/server.  rails defaults to port 3000.

you may need node.js for ruby, otherwise you might need to use ruby racer. or uncomment 

`gem 'mini_racer', platforms: :ruby` 

in the Gemfile

`pages` is a convention for naming controllers. Use it to avoid needing to do other nasty stuff.  

A folder needs to have the same name as the controller for the pages. `app/views/page` for example with 'pages' controller

steps (for making views and pages and routes): 
1. start with route you are interested in and add it to `/app/config/routes.rb`
```ruby
Rails.application.routes.draw do
  # route         controller#action
  root :to => 'pages#homepage'
  get '/hello' => 'pages#hello'
  get '/there' => 'pages#there'

  get '/auto/:color' => 'auto#car' # these are all routes
  get '/nameOfRoute' => 'nameOfRoute#page' # the page will be something like page.html.erb
end
```
the action at the end of these lines is really the name of the function that you have written for the page logic in the `controller.rb`. the controller is the name of the `<name>_controller.rb` file 

2. need to include a file `/app/controllers/nameOfRoute_controller.rb` file, with the contents:
```ruby
class PagesController < ApplicationController
  def hello
      render :hello
    end     
end
```
This is where you write all the page logic/backend. 

3. create the actually page you want to display in the folder `app/views/routeName` witht the kind of name `hello.html.erb` with whatever erb/html you want in it. 

```
create route, 
if not a controller for that route
  create controller
then create page. 
```

### including images

images are sotered in `/app/assets/images/myImg.jpg` but you only need to write `/assets/myImg.jpg`. This is similar to sinatra where you didnt have to mention the `/public` folder, it just knew what you were talking about. 

### including extra gems

Add them into the Gemfile
and write `bundle install` or `bundle` to install the extra gems you have installed. 

### including CSS

add in `/assets/stylesheets`. 

by default, all of the css files will operate on every single page in ruby on rails. it is more advanced to get a particular css file to apply to only certain pages.
#### other notes/conventions

for homepage; instead of using using `get '/'`, you should use `root :to => "pages#homepage"` or whatever else you call the file

in the stylesheets.css file, do not touch the `application.css` file. It is important.

`rails new project --skip-git` to skip creating a git repository. 

Only use POST when you are updating information. When you only want to get information, then use a GET request.

If you arent using a CRUD system, then rails is overkill. 
Sites that are small (not many pages), or dont have a database, or are just a status site are often just sinatra.

if you create an exception in your code (like a breakpoint) then you can use the error page's interactive terminal in rails to explore your variable at that point in time. You can create this exception using `raise "hell"`. Its akin to calling `binding.pry`.



## Common rails errors:
[See even more online](https://rollbar.com/blog/top-10-ruby-on-rails-errors)

* `Unable to autoload constant RockpaperscissorsController, `
likely means you have misspelled the class name in your whatever_controller.rb file. Copy the name they are looking for an paste over you controller's class name to get the right format (capital at start, capitalized 'Controller' at the end).

note you need to create a layout with extension `.html.erb` in the layouts folder for each view that you define. Otherwise, you wont get those lovely headings and footers and whatnot. 

``undefined method `committed?' for nil:NilClass`` this happens when you overwrite the keyword `response`. Dont call anything response. 

comments may still get executed? wtf?  

totally **foobar'd** your install? use `rvm implode` and reinstall. Use implode. Dont try and remove it yourself. 
Also you may need to install rails a couple of times, so it gets added to both `.bash_rc` and `.bbash_profile`.


invalid authemticity token; if you hand write your form, then you need to disable a security feature in the applicationcontroller. 
```
skip_before_action :verify_authenticity_token
```
you can include this on just the route that controls the page you want to disable it for too though. The above is for disabling it globally 
# View-helpers

can often skip some of the render calls by creating views with names that are matching the action's names. 

# CRUD with Rails

See lecture on 16/6 from 12:00  onwards

`.sch` is the same as `.schema`

whenever you update gem file; run `bundle`, and restart the server. 

## Steps: 
### 1. 
`rails db:create` will create a database file for you
### 2. 
run `sqlite3 developement.sqlite3 < create_planets.sql` from inside the db folder
### 3.
 `/app/models` is what we will mess with now. Rails is an mvc (model viewer controller). A MVC usually includes an ORM, but it doesnt have to. It can natively run its own SQL, making it not use an ORM specifically. 
### 4. 
A model matches the plural name of a table in the db folder, but convention is to not use a plural. ie  drop the 's' or whatever. 

``` class Planet < ActiveRecord::Base```

`gem pry-rails` lets you use `rails console` to interactively use rails models in pry. Useful for debugging your database. Seeing if everything is connected. 

## 5. adding seed data

use the file seeds.rb file in db folder. eg
```ruby
Planet.create :name => 'Earth', :orbit = 365.25, :moons => 1, 
```
`rails db:seed` will create the seed data in the database

`use the command Planet.destroy_all` to reset the database before you put in the seed data. It will just stop the seed data being duplicated. WARNING though, this can destroy any other data you have added. 

use `ctrl-p` as a finder, and use it to jump around the all the ruby files, as otherwise it can be a pain. 

`_path` is used to as a post-fix for the route when you want to access its location/path. 



if you make a new rails project with the same name as a deleted one, you can run the command `bin/spring stop` to remove the elements of the old project hidden in the background of ruby


## migration
using the right name will let rails do some of the work for tyou


t is the table helper
rails automatically does the id, when generating tables
active record does the work for us
delete the whole file and recreate if you made a typo 

`rails db:rollback` will undo the latest table you added with db:migrate
can only do so until you add it to git

`schema.rb` is a good place to see the size of the database, `routes.rb` is a good place to see the size of the website. 

schema.rb is READ ONLY. Dont edit stuff there. 

need to add models to interact with the db.


rails db:seed to add data from my seeds.rb file. 

rails console - to interact and play with variables
```ruby 
Rails.application.routes.draw do
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
  resources :artists
end
``` 
will auto build all the routes. check it out at `ruby routes`

```rails generate controller index new edit show```
will create all the views you inputted for you. will also create a stylesheet, and a coffee script
you still need to write all the destroy and whatnot yourself


the scss files will apply site wide, but you are supposed to include the relevant css to a particularly page in the one titled by the file. 

strfti.me for formatting time - very commonly used and old eg `%e %B %y`

`.strftime` is build into ruby. 

there are many ways of using link_to. Lots of cut down versions. You can just pass the object, rather than the id or whatever. Theres are like 4 common shortcut ways of doing it.

create set properties of fields using symbols and hash rockets. 
`:placeholder =>"some string"`


`private` is a keyword that marks the rest of the functions as private. cant be accesed from out of that file. useful for using **strong params**

if you have errors, make sure you restart your server. 