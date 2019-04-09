
# An Introduction to Functions


## Introduction

Functions are a way you can take little bits of your Python scripts and use them multiple times within your programs. As your code gets more complex, they become an increasing important way to hide complexity and reduce duplication in your code. You'll need to use them for the upcoming web scraping lab, and they will also be useful for keeping your code comprehensible, simple and maintainable after this course.

## Objectives
You will be able to:
* Explain the benefits of functions
* Create functions
* Call the created functions

## Introducing Functions

Functions are a way of reusing pieces of code. Let's start with a really simple example. Imagine that we have a group of employees who have just joined our company. We want to send each of them a nice welcome message. We could use a for loop to create a list of welcome_messages.


```python
new_employees = ['jim', 'tracy', 'lisa']
welcome_messages = []
for new_employee in new_employees:
    welcome_messages.append("Hi " + new_employee.title() + ", I'm so glad to be working with you!" )

welcome_messages
```

Then a couple of weeks later, a few more employees join, and we want to send messages to them as well. Well to accomplish welcoming the new employees, we would likely copy our code from above.


```python
new_employees = ['steven', 'jan', 'meryl']
welcome_messages = []
for new_employee in new_employees:
    welcome_messages.append("Hi " + new_employee.title() + ", I'm so glad to be working with you!" )
    
welcome_messages
```

If each time we wanted to reuse code we needed to copy and paste the code, we would have to maintain a lot more code than is necessary. Also, each time we recopied it is another opportunity to make a mistake. So what if there was a way to write that code just one time, yet be able to execute that code wherever and whenever we want? Functions allow us to do just that.

Here is that same code wrapped in a function.


```python
def greet_employees():
    welcome_messages = []
    for new_employee in new_employees:
        welcome_messages.append("Hi " + new_employee.title() + ", I'm so glad to be working with you!" )

    return welcome_messages

greet_employees()
```

There are two steps to using a function: defining a function and executing a function. Defining a function happens first, and afterward when we call greet_employees() we execute the function.


```python
new_employees = ['Jan', 'Joe', 'Avi']
greet_employees()
```

Ok let's break down how to define, or declare, a function. Executing a function is fairly simple, just type the function's name followed by parentheses.


```python
greet_employees()
```

## Declaring and Using Functions

There are two components to declaring a function: the function signature and the function body.


```python
def name_of_function(): # signature
    words = 'function body' # body
    print(words) # body
```

### Function Signature
The function signature is the first line of the function. It follows the pattern of `def`, `function name`, `parentheses`, `colon`.

`def name_of_function():`

The `def` is there to tell Python that you are about to declare a function. The name of the function indicates how to reference and execute the function later. The colon is to end the function signature and indicate that the body of the function is next. The parentheses are important as well, and we'll explain their use in a later lesson.

### Function Body
The body of the function is what the function does. This is the code that runs each time we execute the function. We indicate that we are writing the function body by going to the next line and indenting after the colon. To complete the function body we stop indenting.


```python
def name_of_function(): 
    words = 'function body' # function body
    print(words) # function body    
# no longer part of the function body
```

Let's execute the name_of_function function.


```python
name_of_function()
```

Did it work? Kinda. The lines of our function were run. But our function did not return anything. Functions are designed so that everything inside of them stay inside. So for example, even though we declared the `words` variable, `words` is not available from outside of the function.


```python
words
```

To get something out of the function, we must use the return keyword, followed by what we would like to return. Let's declare another function called other_function that has a body which is exactly the same, but has a return statement.


```python
def other_function(): # signature
    words = 'returned from inside the function body' # body
    return words
```


```python
other_function()
```

Much better. So with the return statement we returned the string `returned from inside the function body`.

### See it again
Now let's identify the function signature and function body of our original function, `greet_empoyees()`.


```python
def greet_employees(): # function signature
    welcome_messages = [] # begin function body
    for new_employee in new_employees:
        welcome_messages.append("Hi " + new_employee.title() + ", I'm so glad to be working with you!" )

    return welcome_messages # return statement

# no longer in function body
```

As you can see, `greet_employees` has the same components of a function we identified earlier: the function signature, the function body, and the return statement. Each time we call, `greet_employees()`, all of the lines in the body of the function are run. However, only the return value is accessible from outside of the function.


```python
greet_employees()
```

## Applying Functions

Let's go back to a problem we solved a few weeks ago. We had a bunch of companies with different company types, but the varying cases caused us a problem when trying to understand how many of the companies were LLC's, etc...


```python
import pandas as pd 
df = pd.read_csv('sample_data.csv')
df['EntityType'].value_counts()
```

One way to solve the problem would be the following code:


```python
df.loc[df["EntityType"] == "llc", "EntityType"] = "LLC"
df.loc[df["EntityType"] == "c corp", "EntityType"] = "C corp"
df.loc[df["EntityType"] == "s corp", "EntityType"] = "S corp"
df['EntityType'].value_counts()
```

But you notice that we're duplicating code. Those three lines of code are really very similar - just changing a different value in the dataframe. It's not too bad with three lines of code, but imagine if we had to write the same code 50 times - and then if something changed in the structure of our data so we had to change all 50 lines of code. Then imagine that those lines of code are in different parts of the notebook and that maybe you forgot to change one or two of them.

This may sound very hypothetical right now (imagine that your office is being ovverrun by zombies, and your only weapon is a stapler, and you're out of staples . . .), but it turns out that unlike a zombie apocalypse, having lots of duplicated code that changes quite often is actually something that happens all the time when you write code.

Functions allow you to pull out repetitive pieces of code to make them simpler to manage, test and modify over time. Lets try this again but using a function.


```python
def replacewith(dataframe, column, find, replace_with):
    dataframe.loc[dataframe[column] == find, column] = replace_with
    return dataframe

df2 = pd.read_csv('sample_data.csv')
df2 = replacewith(df2, "EntityType", "llc", "LLC")
df2 = replacewith(df2, "EntityType", "c corp", "C corp")
df2 = replacewith(df2, "EntityType", "s corp", "S corp")
df2['EntityType'].value_counts()
```

Let's look at what is going on here. Firstly we defined the function, then we said what parameters it took as inputs, then we wrote the code for it to run, and finally we told it what it should "return".

That's a little better. We're replacing the implementation code with the function, but there is still a lot of duplication. I bet a small loop could fix that up:


```python
def replacewith(dataframe, column, find, replace_with):
    dataframe.loc[dataframe[column] == find, column] = replace_with
    return dataframe

replacements = {"llc": "LLC",
                "c corp": "C corp",
                "s corp": "S corp"
               }

df3 = pd.read_csv('sample_data.csv')

for key, value in replacements.items():
    df3 = replacewith(df2, "EntityType", key, value)
df3['EntityType'].value_counts()
```

## Summary


