---
layout: post
title: "Object-Oriented PHP - An Easy Approach"
comments: true
author: "@codehakase"
---
![OOP-PHP]({{site.url}}/assets/oop-in-php.png "Object-Oriented Programming in PHP")

For some PHP developers, the concept of object-orient programming, seems like a frightening concept. You might have browsed through repos on GitHub or read articles on how to implement a feature in a particular PHP project of yours, and the code is full of complicated syntax. I tell you, the concept of OOP is easy to grasp.

**Object-Oriented programming** (OOP), is a style of programming which allows us developers and programmers group related tasks or actions into classes to produce effective code. OOP follows the tenet *"don't repeat yourself (DRY)*, which are common in procedural programming.
## Why Learn OOP?
Many frameworks built upon PHP are using the OOP paradigm and if you intend learning one of those frameworks (Laravel, Codeigniter, Symfony, etc), you'll need a ground on OOP to start with.

## Objects and Classes
In OOP there are two major concepts you'd see often (they really make up OOP), they are **objects** and **classes**. These terms sometimes appear to be interchangeable. That is not the case.
A class for example, stands as a **structure** or *blueprint*. Relate that to a Building, say a Bank, a blueprint is presented which defines the properties of the buildings, and once accepted they proceed to errect the building. *Classes* form the structure of data needed to build objects. That same bank can use the same bluprint and errect same structure in a different location with slight differences (branch manager, number of staff, etc).

### Creating Classes
The syntax to create a class in PHP is very straightforward, the `class` key word is used to declare a class, followed by the name and a pair of curly braces `{ ... }`

```php 
<?php 

class OurClass 
{
  // properties and methods
}
```

So we can write a basic `User` class as such
```php
<?php 
class User
{
  public $firstname;
  public $lastname;

  public function __construct($firstname, $lastname)
  {
    $this->firstname = $firstname;
    $this->lastname = $lastname;
  }

  public function showInfo()
  {
    return $this->firstname . " " . $this->lastname;
  }
}
// using the class
$administrator = new User("John", "Doe");
echo $administrator->showInfo(); // should output 
```

The code above, creates a basic blueprint for a user, the `__construct` method, is a *magic* method. The method is called when ever the class is **instantiated**. The constructor accepts two parameters `$firstname` and `$lastname` and these are passed to the class properties `$this->firsname` and `$this->lastname`.

`$this` refers to the current class in scope.

Also in the `User` class, there's a `showInfo` method, that dumps the data in a raw format.

To instantiate a class, you use the `new` keyword -- ``` $obj = new SomeClass; ```. When a class is instantiated, it creates a copy of the blueprint to that object.
We can have various objects as instance of the same class, but their data will vary.

Add to the code 
```php
<?php 
use Codehakase\User;

$user_a = new User("Francis", "Sunday");
$user_a->showInfo();
echo "<br>";
$user_b = new User("Desiigner", "Doe");
$user_b->showInfo();
```
Load up the script in your favorite browser `localhost/dir/oop.php`, you should see an output similar to this:

```
/var/www/html/tuts/backend/PHP/oop.php:15:
object(User)[1]
  public 'firstname' => string 'John' (length=4)
  public 'lastname' => string 'Doe' (length=3)

/var/www/html/tuts/backend/PHP/oop.php:15:
object(User)[2]
  public 'firstname' => string 'Francis' (length=7)
  public 'lastname' => string 'Sunday' (length=6)

/var/www/html/tuts/backend/PHP/oop.php:15:
object(User)[3]
  public 'firstname' => string 'Desiigner' (length=9)
  public 'lastname' => string 'Doe' (length=3)

```
Awesome!! each instance of the `User` class is independent of the others, their data is not same.


### Method and Property Visibility
Now the `public` keyword, is a visibility type. There are three of them `public`, `protected`, and `private`. Also in addition to a property or method visiblity, there's `static` keyword, which allows them to accessed even without an instantiation of the class.
Any property (variable) or method (class functions) with the `public` keyword, is accessible anywhere, within and outside the class.

Any property or method declared as `protected` can only be accessible within that class and its child classes.

A property or method declared as `private`, is only accessible from within the class it was defined in.

## Comparing Procedural code and Object-Oriented code
*OOP actually provides an easier approach to dealing with data. Because an object can store data internally, variables don't need to be passed from function to function to work properly.*
Consider the following getter, setter procedural code
```php 
<?php
function sample() {
  return [
    //
  ]
}
// a basic setter function 

function sampleSet($sample, $name, $value) {
  $sample['vars'][$name] = $value; // creates a multidimensional array from function arguments
  return $sample; //returns the loaded array
}

// a basic getter 
function sampleGet($sample, $name) {
  if (isset($sample['vars'][$name])) {
    $value = $sample['vars'][$name];
    return [
      $sample,
      $value
    ]
  }

  // using the functions 
  $ex = sample();
  $ex = sampleSet($ex, 'Helo', 'World'); // set an item
  list($ex, $value) = sampleGet($ex, 'World'); // grab an item
}
?>
```

Now i'll re-write that in an OOP style.

```php 
<?php
class Sample
{
  private $vars;

  public function set($name, $value)
  {
    $this->var[$name] = $value;
  }

  public function get($name)
  {
    return isset($this->vars[$name]) ? $this->vars[$name] : null;
  }
}

// using 
$ex = new Sample;
$ex->set("Hello", "World");
$value = $example->get("World");

var_dump($ex);
?>
```
Running the OOP code, you get something similar to:
```
/var/www/html/tuts/backend/PHP/oop.php:49:
object(Sample)[4]
  private 'vars' => null
  public 'var' => 
    array (size=1)
      'Hello' => string 'World' (length=5)
```
### Better Code Management
With OOP, you can manage your files within your application easily, by making use of autoloading.

A typical procedural dependencies script may look as such:
```php 
<?php
require_once "php-includes/connect.php"
require_once "php-includes/do-login.php"
require_once "php-includes/reset-password.php"
require_once "php-includes/do-signup.php"
...
```
We can use class autoloading to include all the files we want
```php
function __autoload($class) {
  include_once 'inc/class' . $class . '.php';
}
```
Preferably, i use [Composer](https://getcomposer.org) and the [PSR-4](https://getcomposer.org/doc/04-schema.md#psr-4) autoloading standard, for everything autoloading.
A basic Autoload with composer would look like this:
```json
{
  "autoload": {
    "psr-4": {
      "Codehakase\\": "app/"
    }
  }
}
```
```php 
<?php 
require_once "vendor/autoload.php" // composer's autoloader

use Codehakase\Models\User;

$user = new User;

$user->learnOOP(true);
...
```

# Conclusion
It's been awesome walking you through the concept of object-orient programming in PHP. Learning or mastering OOP is a really great way to up your programming skills to the next level as a PHP Developer.
We are still gonna continue learning other concepts of OOP, and build a real application with it. All this to come in subsequent articles after this.

Got any questions or suggestions? Drop them in the comment box below.