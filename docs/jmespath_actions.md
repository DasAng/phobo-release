# JMESPath actions

This is a list of actions related to JMESPATH operations.

- [Match](#match)
- [Select](#select)


---

## Match

This action will use an JMESPath query to search and match values from the last saved JSON object from any actions that produced a JSON response.
This could for example be a call to `aws ssm get paramter` or some other actions.

A full comprehensive tutorial on JMESPath is out of scope for this documenation but you can find good tutorials for example here https://jmespath.org/tutorial.html.


`Regex`:

```shell
/jmespath match (.+)/i
```

`match signature`:

```shell
jmespath match <query> 
"""<expected_result>"""
```
- `<query>`: the jmespath query
- `<expected_result>`: the expected value. Can be a JSON or any kind of value.

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Select *foo.bar* and expects the value to be *hello*

    > jmespath match foo.bar      
    > """   
    > hello  
    > """

- Select *foo* and expects the value to be *{ "bar": "hello" }*

    > jmespath match foo  
    > """  
    > { "bar": "hello" }  
    > """

## Select

This action will use an JMESPath query to search and select values from the last saved JSON object from any actions that produced a JSON response.
This could for example be a call to `aws ssm get paramter` or some other actions.

A full comprehensive tutorial on JMESPath is out of scope for this documenation but you can find good tutorials for example here https://jmespath.org/tutorial.html.


`Regex`:

```shell
/jmespath select (.+)/i
```

`match signature`:

```shell
jmespath select <query> 
```
- `<query>`: the jmespath query

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Select *foo.bar* from the last saved result

    > jmespath select foo.bar      
