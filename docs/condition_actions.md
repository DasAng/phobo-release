# Match actions

- [Match result](#match-result)


---

## Match result

This action will perform a match of the last result from a previous action against the specified value using one of the matching operator *less,equal,greater,greater or equal, less or equal*.

`Regex`:

```shell
/match result (not)?\s*(greater or equal|less or equal|less|equal|greater|>=|<=|=|>|<) (.+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
match result [not] <operator> <value>
```

- **`not`**: this parameter is optional and if specified it will negate the matching operator.
- **`operator`**: this parameter is required and can be one of the following values:
    - *less*: will check if the result is less than the specified value
    - *<*: alias for *less*
    - *greater*: will check if the result is greater than the specified value
    - *>*: alias for *greater*
    - *equal*: will check if the result is equal to the specified value
    - *=*: alias for *equal*
    - *less or equal*: will check if the result is less than or equal to the specified value
    - *<=*: alias for *less or equal*
    - *greater or equal*: will check if the result is greater than or equal to the specified value
    - *>=*: alias for *greater or equal*
- **`value`**: this parameter is required and is the value to match against. It can be a number, string, object, array. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter

The result of the action is the boolean value true otherwise it will throw if failed to match

Example of usage:

- Match the result of previous action is equal to number 10

    > match result equal 10
    > match result = 10

- Match the result of previous action is not equal to number 10

    > match result not equal 10
    > match result not = 10

- Match the result of previous action is equal to object { "age": 10 }

    > match result = {"age": 10}
    > match result equal {"age": 10}

- Match the result of previous action is not equal to object { "age": 10 }

    > match result not = {"age": 10}
    > match result not equal {"age": 10}

- Match the result of previous action less than number 10

    > match result less 10
    > match result < 10

- Match the result of previous action less or equal to number 10

    > match result less or equal 10
    > match result <= 10

- Match the result of previous action greater or equal to number 10

    > match result greater or equal 10
    > match result >= 10

- Match the result of previous action equal to array [1,2,{"age": 10}]

    > match result equal [1,2,{"age":10}]
    > match result = [1,2,{"age":10}]