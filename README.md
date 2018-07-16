
# A Deeper Dive Into `Self`

## Introduction
In this lesson, we are going to talk a little more about `self` in object oriented Python (OOP). We have seen a little bit about self when we learned about defining and calling instance methods. What we know so-far is that instance methods implicitly passes in, as its first argument, the instance object on which it was called. We also know that in order for us to execute an instance method, we need to *explicitly* define the instance method with at least one parameter, so that our instance methods can *implicitly* pass in the instance object. By convention, we name this parameter `self` since it is the reference to the object on which we are operating. Let's take a look at some code that uses `self`.

## Objectives
* Using Self
* Operating on Self

# Using Self

In order to really understand self and how it's used, it is best to use an example. Let's use a the example of a **Person** class. A class produces instance objects, and we know that objects are just pieces of code that bundle together attributes like descriptors and behaviors. A Person can have an descriptors like `height`, `weight`, `age`, etc. and also have behaviors such as `saying_hello`, `eat_breakfast`, `talk_about_weather`, etc. 


```python
class Person():
    
    def say_hello(self):
        return "Hi, how are you?"
        
    def eat_breakfast(self):
        return "Yum that was delish!"
        
gail = Person()
gail.name = "Gail"
gail.age = 29
gail.weight = 'None of your business!'
print("1.", gail.say_hello())
print("2.", gail.eat_breakfast())
print("3.", vars(gail))
```

    1. Hi, how are you?
    2. Yum that was delish!
    3. {'name': 'Gail', 'age': 30, 'weight': 'None of your business!'}


Here we can seee that our person instance objects have two behaviors and we can add instance variables and assign values to them pretty easily. What if we wanted a method that introduces oneself? It would be similar to our `say_hello` method, but would probably at least include our name, right? Let's refactor our class to include a `introduce` method.


```python
class Person():
    
    def introduce(self):
        return "Hi, my name is Gail. It is a pleasure to meet you!"
        
gail = Person()
gail.name = "Gail"
the_snail = Person()
the_snail.name = "The Snail"
print("1. ", gail.introduce())
print("2. ", the_snail.introduce())
```

    1.  Hi, my name is Gail. It is a pleasure to meet you!
    2.  Hi, my name is Gail. It is a pleasure to meet you!


Uh oh! That's not quite right. We need our introduce method to be a bit more dynamic for each of the Person instance objects to be able to introduce themselves. This is where self comes in! `self`, after all, is the instance object we are asking to introduce itself. And since our instance method already has our instance object available, we can just interpolate our instance object's name attribute. 

Let's again refactor our class to make this change


```python
class Person():
    
    def introduce(self):
        return f"Hi, my name is {self.name}. It is a pleasure to meet you!"
        
gail = Person()
gail.name = "Gail"
the_snail = Person()
the_snail.name = "the Snail"
print("1. ", gail.introduce())
print("2. ", the_snail.introduce())
```

    1.  Hi, my name is Gail. It is a pleasure to meet you!
    2.  Hi, my name is the Snail. It is a pleasure to meet you!


Great! See how the method is the same for both instance objects, but `self` is not the same. `self` always refers to the object which is being operated on. So, in the case of `gail`, the method returns the string with the `name` attribute of the instance object `gail`. 

Now let's think about some of our other behaviors that might be a bit more involved in order to make them dynamic. For example, everyone's favorite default conversation, the weather. It changes rapidly and seems to always be a choice topic for small talk. How would we create a method to introduce ourselves AND make a comment about the weather? Talk about a great way to introduce yourself!

Let's see how we would do this with just a regular function:


```python
def say_hello_and_weather(instance_obj, weather_pattern):
    return f"Hi, my name is {instance_obj.name}! What crazy {weather_pattern} weather we're having, right?!"

say_hello_and_weather(the_snail, "overcast")
```




    "Hi, my name is the Snail! What crazy overcast weather we're having, right?!"



Alright, that is great but we want it as an instance method, right? So, let's go back to the drawing board with our Person class and see if we can get this awesome new greeting method figured out.


```python
class Person():

    def say_hello_and_weather(self, weather_pattern):
        # we are using self instead of instance_obj because we know self represents the instance object
        return f"Hi, my name is {self.name}! What crazy {weather_pattern} weather we're having, right?!"

the_snail = Person()
the_snail.name = "the Snail"
print("1. ", the_snail.say_hello_and_weather("humid"))
# notice that we are ONLY passing in the weather pattern argument
# instance methods **implicictly** pass in the instance object as the **first** argument
```

    1.  Hi, my name is the Snail! What crazy humid weather we're having, right?!


Awesome, we nailed it! Again, note that the only arguments we pass in are those that come after `self` when we define an instance method's parameters.

Now that we have figured out how to leverage self and even use instance methods with more than just `self` as an argument, let's look at how we can use self to operate on and modify an instance object.

Let's say it is `gail`'s birthday. Gail is 29 and she is turning 30. How do we make sure our instance object reflects that? Well, we can define an instance method that updates `gail`'s age


```python
class Person():

    def happy_birthday(self):
        self.age += 1
        return f"Happy Birthday to {self.name} (aka ME)! Can't believe I'm {self.age}?!"

the_snail = Person()
the_snail.name = "the Snail"
the_snail.age = 29
print("1. ", the_snail.age)
print("2. ", the_snail.happy_birthday())
print("3. ", the_snail.age)
```

    1.  29
    2.  Happy Birthday to the Snail (aka ME)! Can't believe I'm 30?!
    3.  30


Obviously, this method could be improved, but what is important to note is that not only can we use self to *read* attributes from the instance object, but we can also use self inside these methods to change the attributes of the instance object. 

Let's take this a step further and look at how we can call other methods using self. 

Another very important behavior people have is eating. It is something that we all do and it helps prevent us from getting **hangry**, or angry because we're hungry.


```python
class Person():

    def happy_birthday(self):
        self.age += 1
        return f"Happy Birthday to {self.name} (aka ME)! Can't believe I'm {self.age}?!"

the_snail = Person()
the_snail.name = "the Snail"
the_snail.age = 29
```

## Summary

SUMMARY
