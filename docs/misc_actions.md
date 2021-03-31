# Miscellaneous actions

- [Random string](#random-string)
- [Print](#print)
- [Set environment variable](misc_actions.md#set-environment-variable)
- [Export](misc_actions.md#export)
- [Get environment variable](misc_actions.md#get-environment-variable)

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

- **`message`**: this text to print to console. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.

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

- **`name`**: name of environment variable to set. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.
- **`value`**: value of environment variable to set. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.

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

- **`name`**: name of environment variable to set. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.
- **`value`**: value of environment variable to set. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.

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

- **`name`**: name of environment variable to get. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.

The result of the action is the value of the environment variable.

Example of usage:

- Get environment variable named *MYVAR*

    > `get environment variable MYVAR`