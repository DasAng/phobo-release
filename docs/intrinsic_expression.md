# Intrinsic expression

What is an intrinsic expression? It is an expression that allows you to use built-in functions to expand and substitute the current expression.

To better understand this, let's take an example. You have created a gherkin feature file that looks like the following:

```gherkin
Feature: Test feature

    Scenario: Test scenario
        Given aws credentials from environment
        Given aws region eu-central-1
        When aws sfn list functions
        Then aws sfn describe function arn=first_step_function_arn
```

The above scenario will use the AWS Stepfunction service to list all step functions. Then it will retrieve detailed information about the specified step function using the step function arn.

Now suppose you are interested in getting the detailed information of the first step function found after the action `aws sfn list functions`. Since you don't know the *arn* of the first step function retrieved, you would not be able to specify the *arn* in your second statement `aws sfn describe function arn=`.

So the only way to get the *arn* of the step functions is to extract the arn from the result of the `aws sfn list function` action.

Fortunately for us the action `aws sfn list functions` will always save its result internally and it can be accessed by subsequent actions using one of the following expressions:

- `<$result$>`
- `<$jmes.[jmespath]$>`

> `<$result$>` this expression will be substituted or expanded with whatever result that was last saved by any action that has saved the result.

> `<$jmes.[jmespath]$>` this expression use JMESPATH to select the value to be substituted or expanded with whatever result that was last saved by any action that has saved the result

So we know that the result of the action `aws sfn list function` will result in the following JSON result:

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

So in order to get the first step function arn we can use the intrinsic expression `$jmes.stateMachines[0].stateMachineArn`. 

We can then rewrite our gherkin feature file to look like this:

```gherkin
Feature: Test feature

    Scenario: Test scenario
        Given aws credentials from environment
        Given aws region eu-central-1
        When aws sfn list functions
        Then aws sfn describe function arn=<$jmes.stateMachines[0].stateMachineArn$>
```

At runtime the epxression `<$jmes.stateMachines[0].stateMachineArn$>` will use JMESPATH to select the arn of the first step function and return the value.

Another way to achieve the same result would be to use the intrinsic expression `<$result$>`. Now this does not work if we were to just do this:

```gherkin
Then aws sfn describe function arn=<$result$>
```

Since that would at runtime return the last saved result which was the json:

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

So the `<$result$>` cannot be assigned directly to the *arn* since it is not an arn value but a JSON object. Instead we would have to retrieve the arn first by for example using the action `jmespath select stateMachines[0].stateMachineArn`.

So here is how we could rewrite the gherkin feature file to use the `<$result$>` intrinsice expression:

```gherkin
Feature: Test feature

    Scenario: Test scenario
        Given aws credentials from environment
        Given aws region eu-central-1
        When aws sfn list functions
        When jmespath select stateMachines[0].stateMachineArn
        Then aws sfn describe function arn=<$result$>
```

The action `jmespath select` will save its result and since we selected *stateMachines[0].stateMachineArn* it will save the value of the arn in `<$result$>`.

Not all actions and their parameters supports intrinsic expression, so you will have to look up the documentation for each action to see which parameter supports the intrinsic expression.

---

# Intrinsic references

- [<$result$>](#<$result$>)
- [<$jmes$>](#<$jmes.[jmespath]$>)
- [<$userPool$>](#<$userPool.[jmespath]$>)
- [<$cognitoUsers$>](<$cognitoUsers.[jmespath]$>)
- [<$env$>](#<$env.[variable]$>)
- [<$steps$>](#<$steps.[index]$>)

## <$result$>

This intrinsic expression will return whatever value last saved by an action. So if you have multiple actions that produces result then the last action will overwrite the saved result.

You can use this intrinsic expression in various parameters of actions to dynamically specify values at runtime.

## <$jmes.[jmespath]$>

This intrinsic function will use the JMESPATH query specified by `[jmespath]` on the value last saved by an action. This can be used to extract nested fields or complex query on a JSON result. For example to extract *bar* of the following JSON result:

```json
{
    "foo": {
        "bar": "hello"
    }
}
```

You would write the intrinsic expression as follow:

> `<$jmes.foo.bar$>`

## <$userPool.[jmespath]$>

This intrinsic function will use the JMESPATH query specified by `[jmespath]` on AWS Cognito userpools created by the action [`aws cognito create userpool`](aws_cognito_actions.md#create-userpool). This can be used to extract the userpool id or other information.

The userpools created will be an array of the following format:

```json
[
    "AccountRecoverySetting": { 
         "RecoveryMechanisms": [ 
            { 
               "Name": "string",
               "Priority": number
            }
         ]
      },
      "AdminCreateUserConfig": { 
         "AllowAdminCreateUserOnly": boolean,
         "InviteMessageTemplate": { 
            "EmailMessage": "string",
            "EmailSubject": "string",
            "SMSMessage": "string"
         },
         "UnusedAccountValidityDays": number
      },
      "AliasAttributes": [ "string" ],
      "Arn": "string",
      "AutoVerifiedAttributes": [ "string" ],
      "CreationDate": number,
      "CustomDomain": "string",
      "DeviceConfiguration": { 
         "ChallengeRequiredOnNewDevice": boolean,
         "DeviceOnlyRememberedOnUserPrompt": boolean
      },
      "Domain": "string",
      "EmailConfiguration": { 
         "ConfigurationSet": "string",
         "EmailSendingAccount": "string",
         "From": "string",
         "ReplyToEmailAddress": "string",
         "SourceArn": "string"
      },
      "EmailConfigurationFailure": "string",
      "EmailVerificationMessage": "string",
      "EmailVerificationSubject": "string",
      "EstimatedNumberOfUsers": number,
      "Id": "string",
      "LambdaConfig": { 
         "CreateAuthChallenge": "string",
         "CustomEmailSender": { 
            "LambdaArn": "string",
            "LambdaVersion": "string"
         },
         "CustomMessage": "string",
         "CustomSMSSender": { 
            "LambdaArn": "string",
            "LambdaVersion": "string"
         },
         "DefineAuthChallenge": "string",
         "KMSKeyID": "string",
         "PostAuthentication": "string",
         "PostConfirmation": "string",
         "PreAuthentication": "string",
         "PreSignUp": "string",
         "PreTokenGeneration": "string",
         "UserMigration": "string",
         "VerifyAuthChallengeResponse": "string"
      },
      "LastModifiedDate": number,
      "MfaConfiguration": "string",
      "Name": "string",
      "Policies": { 
         "PasswordPolicy": { 
            "MinimumLength": number,
            "RequireLowercase": boolean,
            "RequireNumbers": boolean,
            "RequireSymbols": boolean,
            "RequireUppercase": boolean,
            "TemporaryPasswordValidityDays": number
         }
      },
      "SchemaAttributes": [ 
         { 
            "AttributeDataType": "string",
            "DeveloperOnlyAttribute": boolean,
            "Mutable": boolean,
            "Name": "string",
            "NumberAttributeConstraints": { 
               "MaxValue": "string",
               "MinValue": "string"
            },
            "Required": boolean,
            "StringAttributeConstraints": { 
               "MaxLength": "string",
               "MinLength": "string"
            }
         }
      ],
      "SmsAuthenticationMessage": "string",
      "SmsConfiguration": { 
         "ExternalId": "string",
         "SnsCallerArn": "string"
      },
      "SmsConfigurationFailure": "string",
      "SmsVerificationMessage": "string",
      "Status": "string",
      "UsernameAttributes": [ "string" ],
      "UsernameConfiguration": { 
         "CaseSensitive": boolean
      },
      "UserPoolAddOns": { 
         "AdvancedSecurityMode": "string"
      },
      "UserPoolTags": { 
         "string" : "string" 
      },
      "VerificationMessageTemplate": { 
         "DefaultEmailOption": "string",
         "EmailMessage": "string",
         "EmailMessageByLink": "string",
         "EmailSubject": "string",
         "EmailSubjectByLink": "string",
         "SmsMessage": "string"
      }
]
```

For example to extract *Id* of the following userpool with name *mytestpool*

> `<$userPool.[?Name=='mytestpool']|[0].Id$>`

## <$cognitoUsers.[jmespath]$>

This intrinsic function will use the JMESPATH query specified by `[jmespath]` on AWS Cognito users created by the action [`aws cognito create user`](aws_cognito_actions.md#create-user). This can be used to extract the user's password, username, userpoolid.

The cognito users have the following format:

```json
[
   {
      "userPoolId": "string",
      "username": "string",
      "password": "string"
   }
]
```

For example to extract *password* of the following user with name *bob*

> `<$cognitoUsers.[?username=='bob']|[0].password$>`

## <$env.[variable]$>

This intrinsic function will return the environment variable named `[variable]`. For example if you have an environment variable set like the following:

```shell
export MYVAR=hello
```

Then you can get the value of the variable *MYVAR* by using the expression:

> `<$env.MYVAR$>`

## <$steps.[index]$>

Each steps in a scenario when executed will yield a result, which can be any data types, like a number, string, json object, arrays etc.
The result of a step will be kept internally and can be accessed by another step using the intrinsic expression `<$steps.[index]$>`. If a step does not produce any result then it will yield a result of null.

This intrinsic function will return the result of the executed steps given the `[index]` of the step. For example if you have the following steps:

```gherkin
When aws ssm get parameter myvar
Then export MYVAR=<$steps.0$>
```

Then the index of the first step is 0 and the second step is 1 etc. So in the example case above we retrieve the result of the first step by calling `<$steps.0$>`
