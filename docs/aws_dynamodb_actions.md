# AWS DynamoDB actions

- [SQL query](#sql-query)

---

## SQL query

This action allows you to perform reads and singleton writes on data stored in DynamoDB, using PartiQL

`Regex`:

```shell
/aws dynamodb sql query/i
"""
<sqlstatement>
"""
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws ssm get parameter <path>
"""
<sqlstatement>
"""
```
- **`sqlstatement`**: the sql statement must be inside a docstring. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The reponse will be converted to JSON and have the following structure:

```json
{
   "Items": [ 
      { 
         ....
      },
      {
         ...
      }
   ],
   "NextToken": "string"
}
```

Example of usage:

- Execute a SQL query to select item with a partition key *pk* and sort key *sk*

    > `aws dynamodb sql query`  
    > """  
    > select * from "mytable" where "pk" = 'foo' and 'sk' = 'bar'  
    > """

References:

[ExecuteStatement](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_ExecuteStatement.html)