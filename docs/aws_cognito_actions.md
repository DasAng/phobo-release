# AWS Cognito actions

- [Create user](#create-user)
- [Delete user](#delete-user)
- [Delete users when finished](#delete-users-when-finished)
- [Create userpool](#create-userpool)
- [Delete userpool](#delete-userpool)
- [Set user password](#set-user-password)
- [Initiate auth](#initiate-auth)
- [User login](#user-login)
- [Get id](#get-id)
- [Get OpenId token](#get-openid-token)

---

## Create user

This action will create a new user in the specified user pool.

`Regex`:

```shell
/aws cognito create user: userpoolid=`([^`]+)` username=`([^`]+)`(?=(?:.*desireddeliverymediums=(?<desireddeliverymediums>sms|email|both))?)(?=(?:.*forcealiascreation=(?<forcealiascreation>true|false))?)(?=(?:.*messageaction=(?<messageaction>resend|suppress))?)(?=(?:.*temporarypassword=`(?<temporarypassword>[^`]+)`)?)(?=(?:.*userattributes=`(?<userattributes>[^`]+)`)?)(?=(?:.*validationdata=`(?<validationdata>[^`]+)`)?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence. The optional parameters can appear in any order.

```shell
aws cognito create user: userpoolid=`<userpoolid>` username=`<username>` [prefix=<desireddeliverymediums>] [forcealiascreation=<forcealiascreation>] [messageaction=<messageaction>] [temporarypassword=`<temporarypassword>`] [userattributes=`<userattributes>`] [validationdata=`<validationdata>`]
```

- **`userpoolid`**: the user pool ID for the user pool where the user will be created. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`username`**: the username for the user. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- `desireddeliverymediums`: specify "email" if email will be used to send the welcome message. Specify "sms" if the phone number will be used. Specify "both" if both should be used. This parameter is optional.
- `forcealiascreation`: this parameter is only used if the *phone_number_verified* or *email_verified* attribute is set to **true**. Otherwise, it is ignored.
If this parameter is set to **true** and the phone number or email address specified in the *userattributes* parameter already exists as an alias with a different user, the action call will migrate the alias from the previous user to the newly created user. The previous user will no longer be able to log in using that alias.
If this parameter is set to **false**, the action throws an *AliasExistsException* error if the alias already exists. The default value is **false**. This parameter is optional.
- `messageaction`: set to "resend" to resend the invitation message to a user that already exists and reset the expiration limit on the user's account. Set to "suppress" to suppress sending the message. Only one value can be specified. This parameter is optional
- `temporarypassword`: the user's temporary password. This parameter is optional.
- `userattributes`: an array of name-value pairs that contain user attributes and attribute values to be set for the user to be created. You can create a user without specifying any attributes other than *Username*. For custom attributes, you must prepend the custom: prefix to the attribute name. The format is a JSON object like the following:
    ```json
    UserAttributes: [
        {
        Name: 'STRING_VALUE', /* required */
        Value: 'STRING_VALUE'
        },
        /* more items */
    ]
    ```
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter. This parameter is optional.
- `validationdata`: the user's validation data. This is an array of name-value pairs that contain user attributes and attribute values that you can use for custom validation, such as restricting the types of user accounts that can be registered. For example, you might choose to allow or disallow user sign-up based on the user's domain. The format is a JSON object like the following:
    ```json
    ValidationData: [
        {
        Name: 'STRING_VALUE', /* required */
        Value: 'STRING_VALUE'
        },
        /* more items */
    ]
    ```
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter. This parameter is optional.

The result of the action is a JSON response looking like the following:

```json
{
    "User": {
        "Username": "STRING",
        "Attributes": [
            {
                "Name": "String",
                "Value": "String"
            }
        ],
        "UserCreateDate ": <Date Representation>, 
        "UserLastModifiedDate ": <Date Representation>, 
        "Enabled ": Boolean, 
        "UserStatus": "String",
        "MFAOptions": [
            {
                "DeliveryMedium": "String",
                "AttributeName": "String"
            }
        ]
    }
}
```
Example of usage:

- Create new user named *bob* in the user pool *myuserpool*

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` ``

- Create new user and set delivery mediums to *email*

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` desireddeliverymediums=email``

- Create new user and set forcealiascreation to *true*

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` forcealiascreation=true``

- Create new user and set messageaction to *suppress*

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` messageaction=suppress``

- Create new user and set temporarypassword to *abcde*

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` temporarypassword=`abcde` ``

- Create new user and set userattributes

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` userattributes=`[{"name": "email", "value": "bob@email.com"}]` ``

- Create new user and set validationdata

    > ``aws cognito create user: userpoolid=`myuserpool` username=`bob` validationdata=`[{"name": "email", "value": "bob@email.com"}]` ``


References:

[AdminCreateUSer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html)

## Delete user

This action will delete a user in the specified user pool.

`Regex`:

```shell
/aws cognito delete user: userpoolid=`([^`]+)` username=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence. The optional parameters can appear in any order.

```shell
aws cognito delete user: userpoolid=`<userpoolid>` username=`<username>`
```

- **`userpoolid`**: the user pool ID for the user pool where the user will be deleted. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`username`**: the username of the user to be deleted. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action does not return any JSON result.

Example of usage:

- Delete user named *bob* in the user pool *myuserpool*

    > ``aws cognito delete user: userpoolid=`myuserpool` username=`bob` ``


References:

[AdminDeleteUSer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminDeleteUser.html)

## Delete users when finished

This action will make sure thats users created in the scenario with the action `aws cognito create user` will be deleted at the end of the scenario regardless whether the scenario runs successfully or failed. This can be used for example to clean up all the users created in the scenario.

`Regex`:

```shell
/^delete cognito users when finished$/i
```

`match signature`:

The matching is case insensitive and must match exactly from start to end

```shell
delete cognito users when finished
```
The action does not return any result.

Example of usage:

- Delete all users when the scenario finished regardless or any steps that failed

    > `delete cognito users when finished`

## Create userpool

This action will create a new userpool.

`Regex`:

```shell
/aws cognito create userpool: poolname=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence. The optional parameters can appear in any order.

```shell
aws cognito create userpool: poolname=`<poolname>` <options>
```

- **`poolname`**: the name of the userpool. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`docstring`**: you can pass additional options as a json object in **docstring**. The complete option format looks like the following:

    ```json
    """
    {
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
        "AutoVerifiedAttributes": [ "string" ],
        "DeviceConfiguration": { 
            "ChallengeRequiredOnNewDevice": boolean,
            "DeviceOnlyRememberedOnUserPrompt": boolean
        },
        "EmailConfiguration": { 
            "ConfigurationSet": "string",
            "EmailSendingAccount": "string",
            "From": "string",
            "ReplyToEmailAddress": "string",
            "SourceArn": "string"
        },
        "EmailVerificationMessage": "string",
        "EmailVerificationSubject": "string",
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
        "MfaConfiguration": "string",
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
        "PoolName": "string",
        "Schema": [ 
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
        "SmsVerificationMessage": "string",
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
    }
    """
    ```
    See [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) for further explanation of all the fields.


The result of the action is a JSON response looking like the following:

```json
{
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
   }
}
```
Example of usage:

- Create new userpool named *mytestpool* with no additional options:

    > ``aws cognito create userpool: poolname=`mytestpool`  ``  
    > """  
    > {}  
    > """

- Create new userpool named *mytestpool* with password policies:

    > ``aws cognito create userpool: poolname=`mytestpool`  ``  
    > """  
    > {  
    >    "Policies": {  
    >       "PasswordPolicy": {  
    >            "MinimumLength": 5,  
    >            "RequireLowercase": true,  
    >            "RequireNumbers": false,  
    >            "RequireSymbols": false,  
    >            "RequireUppercase": false,  
    >            "TemporaryPasswordValidityDays": 10  
    >     } } }  
    > }    
    > """

References:

[CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html)

## Delete userpool

This action will delete a userpool

`Regex`:

```shell
/aws cognito delete userpool: userpoolid=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito delete userpool: userpoolid=`<userpoolid>`
```

- **`userpoolid`**: the user pool ID for the user pool where the user will be deleted. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action does not return any JSON result.

Example of usage:

- Delete userpool with id *eu_central_1_sdsd*

    > ``aws cognito delete userpool: userpoolid=`eu_central_1_sdsd` ``


References:

[DeleteUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DeleteUserPool.html)

## Set user password

This action will delete set the password for a cognito user

`Regex`:

```shell
/aws cognito set user password: userpoolid=`([^`]+)` username=`([^`]+)` password=`([^`]+)` permanent=(true|false)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito set user password: userpoolid=`<userpoolid>` username=`<username>` password=`<password>` permanent=<permanent>
```

- **`userpoolid`**: the user pool ID wher the user is located in. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`username`**: the user name. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`password`**: the password for the user. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`permanent`**: *true* if the password is permanent, *false* if it is temporary

The action does not return any JSON result.

Example of usage:

- Set the password to *abcd* for the user named *bob* in the userpool *eu_central_1_sdsd*

    > ``aws cognito set user password: userpoolid=`eu_central_1_sdsd` username=`bob` password=`abcd` permanent=true ``


References:

[AdminSetUserPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminSetUserPassword.html)

## Initiate auth

This action will initiates the authentication flow.

`Regex`:

```shell
/aws cognito initiate auth: authflow=(USER_SRP_AUTH|REFRESH_TOKEN_AUTH|REFRESH_TOKEN|CUSTOM_AUTH|ADMIN_NO_SRP_AUTH|USER_PASSWORD_AUTH|ADMIN_USER_PASSWORD_AUTH) clientid=`([^`]+)`(?=(?:.*authparameters=`(?<authparameters>[^`]+)`)?)(?=(?:.*clientmetadata=`(?<clientmetadata>[^`]+)`)?)(?=(?:.*analyticsendpointid=`(?<analyticsendpointid>[^`]+)`)?)(?=(?:.*encodeddata=`(?<encodeddata>[^`]+)`)?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito initiate auth: authflow=`<authflow>` clientid=`<clientid>` [authparameters=`<authparameters>`] [clientmetadata=`<clientmetadta>`] [analyticsendpointid=`<analyticsendpointid>`] [encodeddata=`<encodeddata>`]
```

- **`authflow`**: the authentication flow for this call to execute. It can be one of the following values:
    - USER_SRP_AUTH
    - REFRESH_TOKEN_AUTH
    - REFRESH_TOKEN
    - CUSTOM_AUTH
    - USER_PASSWORD_AUTH
    - ADMIN_USER_PASSWORD_AUTH
    
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`clientid`**: the app client ID. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- `authparameters`: the authentication parameters. These are inputs corresponding to the *authflow* that you are invoking. The required values depend on the value of *authflow*. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- `clientmetadata`: a map of custom key-value pairs that you can provide as input for certain custom workflows that this action triggers. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- `analyticsendpointid`: the Amazon Pinpoint analytics metadata for collecting metrics for InitiateAuth calls. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- `encodeddata`: contextual data such as the user's device fingerprint, IP address, or location used for evaluating the risk of an unexpected event by Amazon Cognito advanced security. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action returns a JSON result:

```json
{
   "AuthenticationResult": { 
      "AccessToken": "string",
      "ExpiresIn": number,
      "IdToken": "string",
      "NewDeviceMetadata": { 
         "DeviceGroupKey": "string",
         "DeviceKey": "string"
      },
      "RefreshToken": "string",
      "TokenType": "string"
   },
   "ChallengeName": "string",
   "ChallengeParameters": { 
      "string" : "string" 
   },
   "Session": "string"
}
```

Example of usage:

- Initiates authentication flow to login user *testuser* with password *_12abcdefG*

    > ``aws cognito initiate auth: authflow=USER_PASSWORD_AUTH clientid=`sdaxwa` authparameters=`{"USERNAME": "testuser", "PASSWORD": "_12abcdefG"}` ``


References:

[InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html)

## User login

This action will sign in the cognito user using username and password flow.

`Regex`:

```shell
/aws cognito user login: clientid=`([^`]+)` username=`([^`]+)` password=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito user login: clientid=`<clientid>` username=`<username>` password=`<password>`
```

- **`clientid`**: the app client ID. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`username`**: user name. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`password`**: user password. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action returns a JSON result:

```json
{
   "AuthenticationResult": { 
      "AccessToken": "string",
      "ExpiresIn": number,
      "IdToken": "string",
      "NewDeviceMetadata": { 
         "DeviceGroupKey": "string",
         "DeviceKey": "string"
      },
      "RefreshToken": "string",
      "TokenType": "string"
   },
   "ChallengeName": "string",
   "ChallengeParameters": { 
      "string" : "string" 
   },
   "Session": "string"
}
```

Example of usage:

- login user *testuser* with password *_12abcdefG*

    > ``aws cognito user login: clientid=`sdaxwa` username=`testuser` password=`_12abcdefG` ``


References:

[InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html)

## Get id

This action will generate or retrieve the identity id for a linked cognito user pool.

`Regex`:

```shell
/aws cognito get id: identitypoolid=`([^`]+)` accountid=`([^`]+)` login=`([^`]+)` idtoken=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito get id: identitypoolid=`<identitypoolid>` accountid=`<accouuntid>` login=`<login>` idtoken=`<idtoken>`
```

- **`identitypoolid`**: the app identity pool id. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`accouuntid`**: the aws account id. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`login`**: A set of optional name-value pairs that map provider names to provider tokens. The available provider names for Logins are as follows:

    - Facebook: graph.facebook.com

    - Amazon Cognito user pool: cognito-idp.<region>.amazonaws.com/<YOUR_USER_POOL_ID>, for example, cognito-idp.us-east-1.amazonaws.com/us-east-1_123456789.

    - Google: accounts.google.com

    - Amazon: www.amazon.com

    - Twitter: api.twitter.com

    - Digits: www.digits.com.
    
    You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

- **`idtoken`**: the id token. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action returns a JSON result:

```json
{
   "IdentityId": "string"
}
```

Example of usage:

- get identity id for the identity pool *eu_adwxq12d2*

    > `` aws cognito get id: identitypoolid=`eu_adwxq12d2` accountid=`123456` login=`cognito-idp.eu-central-1.amazonaws.com/<$env.USER_POOL_ID$>` idtoken=`zxsaffcwefgbfvadwadaxwxax` ``


References:

[GetId](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetId.html)

## Get OpenId token

This action gets an OpenID token, using a known Cognito ID. This known Cognito ID is returned by [GetId](#get-id) action.

`Regex`:

```shell
/aws cognito get openid token: identityid=`([^`]+)` login=`([^`]+)` idtoken=`([^`]+)`/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence

```shell
aws cognito get openid token: identityid=`<identityid>` login=`<login>` idtoken=`<idtoken>`
```

- **`identityid`**: a unique identifier in the format REGION:GUID. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`idtoken`**: the aws account id. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`login`**: a set of optional name-value pairs that map provider names to provider tokens. When using graph.facebook.com and www.amazon.com, supply the access_token returned from the provider's authflow. For accounts.google.com, an Amazon Cognito user pool provider, or any other OpenID Connect provider, always include the **idtoken**. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

- **`idtoken`**: the id token. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The action returns a JSON result:

```json
{
   "IdentityId": "string",
   "Token": "string"
}
```

Example of usage:

- get OpenID token for OpenID connect provider

    > `` aws cognito get openid token: identityid=`eu-central-1:234sxx2-22s2-xsds` login=`cognito-idp.eu-central-1.amazonaws.com/<$env.USER_POOL_ID$>` idtoken=`swswaaxxw23dax2` ``


References:

[GetOpenIdToken](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetOpenIdToken.html)