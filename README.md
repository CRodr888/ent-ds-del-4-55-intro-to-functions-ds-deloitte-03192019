
# An Introduction to Functions


## Introduction

Functions are a way you can take little bits of your Python scripts and use them multiple times within your programs. As your code gets more complex, they become an increasing important way to hide complexity and reduce duplication in your code. You'll need to use them for the upcoming web scraping lab, and they will also be useful for keeping your code comprehensible, simple and maintainable after this course.

## Objectives
You will be able to:
* Explain the benefits of functions
* Create functions
* Call the created functions

## Introducing Functions

Functions are a way of reusing pieces of code.

Let's go back to a problem we solved a few weeks ago. We had a bunch of companies with different company types, but the varying cases caused us a problem when trying to understand how many of the companies were LLC's, etc...


```python
import pandas as pd 
df = pd.read_csv('sample_data.csv')
df['EntityType'].value_counts()
```




    C corp    5
    c corp    4
    llc       3
    LLC       3
    S corp    2
    s corp    1
    Name: EntityType, dtype: int64



One way to solve the problem would be the following code:


```python
df.loc[df["EntityType"] == "llc", "EntityType"] = "LLC"
df.loc[df["EntityType"] == "c corp", "EntityType"] = "C corp"
df.loc[df["EntityType"] == "s corp", "EntityType"] = "S corp"
df['EntityType'].value_counts()
```




    C corp    9
    LLC       6
    S corp    3
    Name: EntityType, dtype: int64



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




    C corp    9
    LLC       6
    S corp    3
    Name: EntityType, dtype: int64



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




    C corp    9
    LLC       6
    S corp    3
    Name: EntityType, dtype: int64



## Summary


