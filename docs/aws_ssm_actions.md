# AWS SSM actions

- [Get parameter](#get-parameter)

---

## Get parameter

This action will retrieve a parameter from the SSM parameter store.

`Regex`:

```shell
/aws ssm get parameter (.+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws ssm get parameter <path>
```
- **`path`**: the parameter store path to retrieve the value from. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter.

The reponse JSON will have the following structure:

```json
{
   "Parameter": { 
      "ARN": "string",
      "DataType": "string",
      "LastModifiedDate": number,
      "Name": "string",
      "Selector": "string",
      "SourceResult": "string",
      "Type": "string",
      "Value": "string",
      "Version": number
   }
}
```

Example of usage:

- Get the value for the path */ui/mydata*

    > `aws ssm get parameter /ui/mydata`

References:

[GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)