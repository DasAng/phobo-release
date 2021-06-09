# AWS STS actions

- [Assume role with web identity](#assume-role-with-web-identity)
- [Get caller identity](#get-caller-identity)
- [Get account id](#get-account-id)

---

## Assume role with web identity

This action will returns a set of temporary security credentials for users who have been authenticated in a mobile or web application with a web identity provider. Example providers include Amazon Cognito, Login with Amazon, Facebook, Google, or any OpenID Connect-compatible identity provider.

`Regex`:

```shell
/aws sts assume role with web identity: rolearn=`([^`]+)` sessionname=`([^`]+)` token=`([^`]+)`\s*(?=(?:.*duration=`(?<duration>\d+)`)?)(?=(?:.*providerid=`(?<providerid>[^`]+)`)?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws sts assume role with web identity: rolearn=`<rolearn>` sessionname=`<sessionname>` token=`<token>` [duration=`<duration>`] [providerid=`<providerid>`]
```
- **`rolearn`**: the Amazon Resource Name (ARN) of the role that the caller is assuming. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`sessionname`**: an identifier for the assumed role session. Typically, you pass the name or identifier that is associated with the user who is using your application. That way, the temporary security credentials that your application will use are associated with that user. This session name is included as part of the ARN and assumed role ID in the AssumedRoleUser response element.. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`token`**: the OAuth 2.0 access token or OpenID Connect ID token that is provided by the identity provider. Your application must get this token by authenticating the user who is using your application with a web identity provider before the application calls this action. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.
- **`duration`**: by default, the temporary security credentials created last for one hour. However, You can provide a value from 900 seconds (15 minutes) up to the maximum session duration setting for the role. This setting can have a value from 1 hour to 12 hours.
- **`providerid`**: the fully qualified host component of the domain name of the identity provider. This parameter is optional. Specify this value only for OAuth 2.0 access tokens. Currently www.amazon.com and graph.facebook.com are the only supported identity providers for OAuth 2.0 access tokens. Do not include URL schemes and port numbers. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter.

The reponse JSON will have the following structure:

```json
{
   "Credentials": {
      "AccessKeyId": "string",
      "SecretAccessKey": "string",
      "SessionToken": "string",
      "Expiration": date,
   },
   "SubjectFromWebIdentityToken": "string",
   "AssumedRoleUser": {
      "AssumedRoleId": "string",
      "Arn": "string",
   },
   "PackedPolicySize": number,
   "Provider": "string",
   "Audience": "string",
   "SourceIdentity": "string"
}
```

Example of usage:

- Assume role with arn test-role

    > ``aws sts assume role with web identity: rolearn=`arn:aws:iam::1111111111:role/test-role` sessionname=`test-role-session` token=`123sxawda22sxw` ``

References:

[AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)

## Get caller identity

This action will Returns details about the IAM user or role whose credentials are used to call the operation.

`Regex`:

```shell
/aws sts get caller identity/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws sts get caller identity
```
The reponse JSON will have the following structure:

```json
{
    "UserId": "AIDASAMPLEUSERID",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/DevAdmin"
}
```

Example of usage:

- get identity of current IAM user

    > ``aws sts get caller identity ``

References:

[GetCallerIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetCallerIdentity.html)

## Get account id

This action will return the AWS Account Id

`Regex`:

```shell
/aws sts get account id/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws sts get account id
```
The reponse is a string containing the account id

```json
"123456789012"
```

Example of usage:

- get current AWS account id

    > ``aws sts get account id ``
