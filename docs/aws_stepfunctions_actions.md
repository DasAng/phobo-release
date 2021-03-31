# AWS Stepfunctions actions

- [List step functions](a#list-step-functions)
- [Describe step function](#describe-step-function)

---

## List step functions

This action will list all existing step functions.

`Regex`:

```shell
/aws sfn list functions\s*(?=(?:.*maxresults=(?<maxresults>\d+))?)(?=(?:.*fetchall=(?<fetchall>true|false))?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence. The optional parameters can appear in any order.

```shell
aws sfn list functions [maxresults=<maxresults>] [fetchall=<fetchall>]
```

- `maxresults`: sets the maximum number of functions returned in the response. By default the API returns up to 100 functions. The maximum value is 1000. The response might contain fewer objects but will never contain more. This parameter is optional
- `fetchall`:  this value can either be *true* or *false*. If true then all objects will be fetched if not it will fetch only the specified objects up to the specified *maxresults* or 1000. This parameter is optional.

The result of the action is a JSON response looking like the following:

```json
{
    "stateMachines": [
        {
            "stateMachineArn": "arn:aws:states:eu-central-1:111111111:stateMachine:mystatemachine",
            "name": "mystatemachine",
            "type": "STANDARD",
            "creationDate": "2020-11-25T13:42:43.537Z"
        },
        {
            "stateMachineArn": "arn:aws:states:eu-central-1:1111111:stateMachine:mystatemachine2",
            "name": "mystatemachine2",
            "type": "STANDARD",
            "creationDate": "2020-11-25T13:42:43.747Z"
        },
    ]
}
```

Example of usage:

- List all step functions with default up to max 100

    > `aws sfn list functions`

- List up to max 2 step functions 

    > `aws sfn list functions maxresults=2`

- List up to max 2 step functions per call but retrieve all functions

    > `aws sfn list functions maxresults=2 fetchall=true`

- List all step functions

    > `aws sfn list functions fetchall=true`

References:

[ListStateMachines](https://docs.aws.amazon.com/step-functions/latest/apireference/API_ListStateMachines.html)

## Describe step function

This action will retrive detailed information for a given step function.

`Regex`:

```shell
/aws sfn describe function arn=(.+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws sfn describe function arn=<arn>
```

- `arn`: the arn of the step function. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.


The result of the action is a JSON response looking like the following:

```json
{
  "stateMachineArn": "arn:aws:states:eu-central-1:111111:stateMachine:mystatemachine",
  "name": "mystatemachine",
  "status": "ACTIVE",
  "definition": "{\n    \"StartAt\": \"Get Parameters\"  }\n    }\n}",
  "roleArn": "arn:aws:iam::11111:role/mystaterole",
  "type": "STANDARD",
  "creationDate": "2020-11-25T13:42:43.537Z",
  "loggingConfiguration": {
    "level": "OFF",
    "includeExecutionData": false
  },
  "tracingConfiguration": {
    "enabled": false
  }
}
```
Example of usage:

- Describe the step function with the arn *arn:aws:states:eu-central-1:111111:stateMachine:mystatemachine*

    > `aws sfn describe function arn=arn:aws:states:eu-central-1:111111:stateMachine:mystatemachine`

References:

[DescribeStatMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_DescribeStateMachine.html)