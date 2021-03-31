# AWS Credentials actions

- [Get credentials from environment](#get-credentials-from-environment)
- [Get credentials from configuration file](#get-credentials-from-configuration-file)
- [Set credentials](#set-credentials)
- [Set region](#set-region)

---

## Get credentials from environment

This action will retrieve the AWS credentials from environment variables. The environment variables expected are:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

This action will fail if the environment variables are not found.

`Regex`:

```shell
/aws credentials from environment/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws credentials from environment
```

Example of usage:

- Get the AWS credentials from environment variables

    > `aws credentials from environment`

## Get credentials from configuration file

This action will retrieve the AWS credentials shared init file usually located in the following location:

- The shared credentials file on Linux, Unix, and macOS: ~/.aws/credentials
- he shared credentials file on Windows: C:\Users\USER_NAME\.aws\credentials

You can optionally specify a profile from the configuration file to use.

This action will fail if the environment variables are not found.

`Regex`:

```shell
/aws credentials from config\s*(?:profile=(.+))?/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws credentials from config [profile=<profilename>]
```
- `profilename`: an optional profile name to use. This parameter is optional.


Example of usage:

- Get the AWS credentials from configuration file using default profile:

    > `aws credentials from config`

- Get the AWS credentials from configuration file using specified profile 'myprofile':

    > `aws credentials from config profile=myprofile`


## Set credentials

This action will set the AWS credentials to the specified values.

This action will fail if the credentials are empty.

`Regex`:

```shell
/aws credentials: accesskey=([^\s]+)\s*secretkey=([^\s]+)\s*(?:sessiontoken=([^\s]+))?/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
aws credentials: accesskey=<aws_access_key> secretkey=<aws_secret_access_key> [sessiontoken=<session_token>]
```
- **`aws_access_key`**: the aws access key to set
- **`aws_secret_access_key`**: the aws secret key to set
- `session_token`: an optional session token value to set

Example of usage:

- Set the aws access key and secret key, but not session token:

    > `aws credentials: accesskey=AIBSZWDF secretkey=/sdasds#xa12dsad`

- Set the aws access key, secret key and session token:

    > `aws credentials: accesskey=AIBSZWDF secretkey=/sdasds#xa12dsad sessiontoken=|sdadasdsdsad%sad2`

## Set region

This action will set the AWS region to use.

This action will fail if the region is empty.

`Regex`:

```shell
/aws region\s*(.+)/i
```

`match signature`:

```shell
aws region <region>
```
- `<region>`: the region value, fx: *eu-central-1* or *eu-west-1* etc.

The matching is case insensitive and can appear anywhere within the sentence.

Example of usage:

- Set the aws region to *us-east-1*:

    > `aws region us-east-1`