# Miscellaneous actions

- [Random string](#random-string)
- [Print](#print)
- [Set environment variable](misc_actions.md#set-environment-variable)
- [Export](misc_actions.md#export)
- [Get environment variable](misc_actions.md#get-environment-variable)
- [Variable](#variable)
- [Var](#var)
- [Copy to clipboard](#copy-to-clipboard)

---

## Random string

This action will generate a random string with the specified length.

`Regex`:

```shell
/random string (\d+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
random string <length>
```

- **`length`**: this parameter is required and is the length of the random string to generate.

The result of the action is a random string with the specified length:

```json
basdsdub.-dasds#
```

Example of usage:

- Make a random string of length 5

    > `random string 5`

## Print

This action will print the specified message to console.

`Regex`:

```shell
/print `([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
print `<message>`
```

- **`message`**: this text to print to console. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the message to print.

Example of usage:

- Print *hello world*

    > ``print `hello world` ``

- Print the environment variable *MYVAR*

    > ``print `$env.MYVAR` ``

## Set environment variable

This action will set the environment variable to the specified value.

`Regex`:

```shell
/set environment variable: name=`([^`]+)` value=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
set environment variable: name=`<name>` value=`<value>`
```

- **`name`**: name of environment variable to set. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`value`**: value of environment variable to set. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the value of the environment variable.

Example of usage:

- Set environment variable named *MYVAR* to the value *hello*

    > ``set environment variable: name=`MYVAR` value=`hello` ``

## Export

This action will set the environment variable to the specified value.

`Regex`:

```shell
/export ([^=]+)=(.+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
export <name>=<value>
```

- **`name`**: name of environment variable to set. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`value`**: value of environment variable to set. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the value of the environment variable.

Example of usage:

- Set environment variable named *MYVAR* to the value *hello*

    > ``export MYVAR=hello ``

## Get environment variable

This action will get the environment variable.

`Regex`:

```shell
/get environment variable (.+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
get environment variable <name>
```

- **`name`**: name of environment variable to get. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the value of the environment variable.

Example of usage:

- Get environment variable named *MYVAR*

    > `get environment variable MYVAR`


## Variable

This action will create a local variable internally that can be referred and used by other actions. This variable must have a unique name.
The value of the variable can be any type, string, number, boolean or JSON object.

`Regex`:

```shell
/^variable name=`([^`]+)` value=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and must appear at the beginning.

```shell
variable name=`<name>` value=`<value>`
```

- **`name`**: name of variable to create. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`value`**: value of variable. The value can be of the following types:
    - string
    - number
    - boolean
    - JSON obejct
    
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the value of the variable.

Example of usage:

- Create a variable named *bob* with JSON value *{"name": "bob"}*

    > ``variable name=`bob` value=`{"name": "bob"}` ``

- Create a variable named *age* with value of 10

    > ``variable name=`age` value=`10` `` 

- Create a variable named *todos* with an array of *["shopping", "studying"]*

    > ``variable name=`todos` value=`["shopping","studying"]` `` 

- Create a variable named *address* with an value of *"sesam street 14"*

    > ``variable name=`address` value=`sesam street 14` `` 

- Create a variable named *location* with a value from another variable named *home*

    > ``variable name=`location` value=`<$var.home$>` `` 


## Var

This action will create a local variable internally that can be referred and used by other actions. This variable must have a unique name.
The value of the variable can be any type, string, number, boolean or JSON object.

`Regex`:

```shell
/^var ([^=]+)=(.+)/i
```

`match signature`:

The matching is case insensitive and must appear at the beginning.

```shell
var <name>=<value>
```

- **`name`**: name of variable to create. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`value`**: value of variable. The value can be of the following types:
    - string
    - number
    - boolean
    - JSON obejct
    
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the value of the variable.

Example of usage:

- Create a variable named *bob* with JSON value *{"name": "bob"}*

    > ``var bob={"name": "bob"} ``

- Create a variable named *age* with value of 10

    > ``var age=10 `` 

- Create a variable named *todos* with an array of *["shopping", "studying"]*

    > ``var todos=["shopping","studying"] `` 


## Copy to clipboard

This action will copy the specified text value to the clipboard

`Regex`:

```shell
/copy to clipboard value=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and must appear at the beginning.

```shell
copy to clipboard value=`<value>`
```

- **`value`**: value to copy to clipboard. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The result of the action is the copied value.

Example of usage:

- Copy the value "hello" to the clipboard

    > ``copy to clipboard value=`hello` ``
