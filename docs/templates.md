# Templates

Templates or more precisly substitution files is a special feature of Phobo that allows it to prettify or transform a Gherkin feature file by replacing Gherkin steps with another expression.

To better illustrate the purpose of having templates let's take an example.

For example you have written a Gherkin feature file looking like the following:

```gherkin
Feature: Test Sign in to website

    Scenario: Sign in to mywebsite.com

        Given a new headless edge browser
        When navigate to https://mywebsite.com
        And I click on sign in button
        Then I am signed in to mywebsite.com
```

Now if you look at the Phobo [API](api.md) you can see that it does not have any actions for the following steps:

- `I click on sign in button`
- `I am signed in to mywebsite.com`

Those two expression are easy to understand for a normal person but for Phobo it does not convey any meaning, since Phobo is a generic tool it does not know any context related to those two statements.

So those two are very context specific statements that requires knowledge about the mywebsite html structure in order to find the sign in button and click on it and determine that you have been signed in successfully.

Now we need to rewrite those two statements into actions that Phobo understand and support.

So we can see from the [API](api.md) that Phobo supports an action named `page click` which will perform a click on an element on the page. We can use this to rewrite the statement `I click on sign in button` to a more specific statement like the following:

> `page click //*button[@id='sign in']`

This will assume that we have looked at how the mywebsite.com HTML page looked like and there is a button with the id of *sign in* on the page so that we can click on it.

Now imagine that the mywebsite.com once signed in will redirect you to the url mywebsite.com/user?name=.

Now we could rewrite the statement `I am signed in to mywebsite.com` with the Phobo action `page url like`:

> `page url like mywebsite.com/user?name=`

So here is the complete rewrite of the feature file:

```gherkin
Feature: Test Sign in to website

    Scenario: Sign in to mywebsite.com

        Given a new headless edge browser
        When navigate to https://mywebsite.com
        And page click //*button[@id='sign in']
        Then page url like mywebsite.com/user?name=
```

So with the new feature file it will allow Phobo to know exactly what to do and how to do it.

Now the issue is that our new feature file is not as "natural" as our original feature file. It has statements that are more tech-savy and not as human friendly as the original feature file.

What if the requirement is that we need to use the original feature file so that other who reads it can understand it easily.

There are two ways to do this

1. Write a custom plugin for Phobo that handles the missing statements
2. Use template files

Now this section is about template files so let's not choose option 1 but rather go for option 2.

So how can template files help us achieve our goal?

Basically template files are external JSON files that specify the step statements that should be replaced with another text. It looks like the following:

```json
{
    "I click on sign in button": "page click //*button[@id='sign in']",
    "I am signed in to mywebsite.com" : "page url like mywebsite.com/user?name="
}
```

So each item in the json file consist of a key and a value:

- key: "I click on sign in button"
  
  value: "page click //*button[@id='sign in']"

- key: "I am signed in to mywebsite.com"
  
  value: "page url like mywebsite.com/user?name="

Phobo can use the template files to transform the original feature file into another feature file and replacing any step statements that matches any key in the template files with the value.

So how does Phobo know which template files to use?

Phobo uses a transformation process to parse the Gherkin feature file looking at all the comments inside the feature file. If it encounter a line starting with the following comment:

> #template: |template file name|

where **|tempalte file name|** is the filename of the template JSON file. For example:

> #template: mytemplate.json

Then it will load the specified tempalte file, fx mytemplate.json. You can have as many template files statements in the feature file and Phobo will load all of them and merging them into one, overwriting any overlapping keys with the last merged template file.

So lets say we have a json file named mywebsite.json looking like the following:

*mywebsiste.json*

```json
{
    "I click on sign in button": "page click //*button[@id='sign in']",
    "I am signed in to mywebsite.com" : "page url like mywebsite.com/user?name="
}
```

We can add the rewrite our feature file to look like this:

```gherkin
#template: mywebsite.json
Feature: Test Sign in to website

    Scenario: Sign in to mywebsite.com

        Given a new headless edge browser
        When navigate to https://mywebsite.com
        And I click on sign in button
        Then I am signed in to mywebsite.com
```

When running this feature file against Phobo it will parse the feature file and uses the *mywebsite.json* template file to transform the feature file into the following:

```gherkin
Feature: Test Sign in to website

    Scenario: Sign in to mywebsite.com

        Given a new headless edge browser
        When navigate to https://mywebsite.com
        And page click //*button[@id='sign in']
        Then page url like mywebsite.com/user?name=
```

And the produced HTML report will report the steps from the original feature file and not the transformed feature file. This way the HTML report will still look human readable and matches the original feature file, even though it is the transformed feature file that is being executed by Phobo.

By using this method Phobo also ensures that it is not being intrusive and require changes to the gherkin language but instead utilizes comments to extend functionality of the original feature file.

This way the original feature file can still be used with other gherkin tools if you do not wish to use Phobo.

Template files can also be used to share commonly known actions that are used by multiple feature files and therefore help you organize actions into specific templates.

The disadvantage of using templates is that you will have to write and maintain another file, i.e the template file and it will only work if you have that file available. Otherwise the feature file will have undefined statements that does not yield any actions.

For small project it can help improve readability but for larger projects with many feature files it can get quite complex to have to maintain and organize all these template files.