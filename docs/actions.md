# Actions reference

Phobo parses and executes Gherkin feature files. Based on the scenarios and steps defined in the feature files specific actions will be performed.

This is a comprehensive list of actions offered by Phobo.


# Browser automation actions

**Browser**

 - [Launch browser](browser_actions.md#launch-browser)
 - [Close browser](browser_actions.md#close-browser)

**Page**
 - [Navigate to url](browser_actions.md#navigate-to-url)
 - [Take screenshot](browser_actions.md#take-screenshot)
 - [Any request error](browser_actions.md#any-request-error)
 - [Page wait](browser_actions.md#page-wait)
 - [Page click](browser_actions.md#page-click)
 - [Page type](browser_actions.md#page-type)
 - [Page query](browser_actions.md#page-query)
 - [Page count queried items](browser_actions.md#page-count-queried-items)
 - [Page queried items contains any,all or exact](browser_actions.md#page-queried-items-contains-not,any,all-or-exact)
 - [Page url like](browser_actions.md#page-url-like)
 - [Page reload](browser_actions.md#page-reload)
 - [Page select dropdown](browser_actions.md#page-select-dropdown)
 - [Page select by text dropdown](browser_actions.md#page-select-by-text-dropdown)
 - [Page element is visible or hidden](browser_actions.md#page-element-is-visible-or-hidden)
 - [Page clear localstorage](browser_actions.md#page-clear-localstorage)
 - [Page delete cookie](browser_actions.md#page-delete-cookie)
 - [Page set value](browser_actions.md#page-set-value)


# AWS actions

**Credentials**

- [Get credentials from environment](aws_credentials_actions.md#get-credentials-from-environment)
- [Get credentials from configuration file](aws_credentials_actions.md#get-credentials-from-configuration-file)
- [Set credentials](aws_credentials_actions.md#set-credentials)
- [Set region](aws_credentials_actions.md#set-region)

**SSM**

- [Get parameter](aws_ssm_actions.md#get-parameter)

**S3**

- [List objeccts](aws_s3_actions.md#list-objects)

**Step functions**

- [List step functions](aws_stepfunctions_actions.md#list-step-functions)
- [Describe step function](aws_stepfunctions_actions.md#describe-step-function)

**Cognito**

- [Create user](aws_cognito_actions.md#create-user)
- [Delete user](aws_cognito_actions.md#delete-user)
- [Delete users when finished](aws_cognito_actions.md#delete-users-when-finished)
- [Create userpool](aws_cognito_actions.md#create-userpool)
- [Delete userpool](aws_cognito_actions.md#delete-userpool)
- [Set user password](aws_cognito_actions.md#set-user-password)
- [Initiate auth](aws_cognito_actions.md#initiate-auth)
- [User login](aws_cognito_actions.md#user-login)

**DynamoDB**

- [SQL query](aws_dynamodb_actions.md#sql-query)

# JMESPath actions

- [Match](jmespath_actions.md#match)
- [Select](jmespath_actions.md#select)

# Miscellaneous actions

- [Random string](misc_actions.md#random-string)
- [Print](misc_actions.md#print)
- [Set environment variable](misc_actions.md#set-environment-variable)
- [Export](misc_actions.md#export)
- [Get environment variable](misc_actions.md#get-environment-variable)

# Conditional actions

- [Repeat until](condition_actions.md#repeat-until)

# Match actions

- [Match result](match_actions.md#match-result)