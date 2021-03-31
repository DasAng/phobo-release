![Phobo](docs/phobo.png)
# Phobo

**Phobo** is a tool for running automated tests written in plain language. By writing tests in a natural language it is much easier to read and understand.
It will help improve collaboration on the test and make it much easier to communicate the test to other people.

Phobo is a standalone executable that can be run from the command line and it will generate a HTML report of the executed tests.

# Documentation

- [Install](#install)
- [Application usage](#application-usage)
    - [CLI](docs/cli.md)
    - [Configuration file](docs/config.md)
    - [Actions references](docs/actions.md)
        - [Browser actions](docs/browser_actions.md)
        - [AWS actions](docs/actions.md#aws-actions)
        - [JMESPATH actions](docs/jmespath_actions.md)
        - [Intrinsic expression](docs/intrinsic_expression.md)
    - [Plugins](docs/plugins.md)
        - [API reference](docs/plugin_api.md)
    - [Migoi - HTML report](docs/migoi.md)
    - [Templates](docs/templates.md)
    - [Tags](#tags)

---

# Install

Phobo is available as a standalone executable for the following platforms:

- Windows
- Linux
- Mac

# Application usage

To run Phobo open a commandline or terminal and type the following

*Windows*

```shell
phobo.exe <feature file>
```

Example:
```shell
phobo.exe test.feature
```

*Linux*

```shell
./phobo <feature file>
```

Example:
```shell
./phobo test.feature
```


Where *feature file* is the feature file you wish to run. The result will be a HTML file generated in subdirectory named *out*. The HTML will look something like the following:

<img src="docs/migoi_overview.png" />

For additional information about the various CLI options available take a look at

- [CLI](docs/cli.md)

# Tags

Tags are a way to attach context to a feature or scenario. Tags are just text values that can have any meaning for a feature or scenario.
To add tags to a scenario or feature you will use the Gherkin tag syntax like for example:

```gherkin
@mytag @anothertag
Feature: This is a test feature
    
    @tagforscenario @anothertag
    Scenario: Test scenario
        
```

The example above adds 2 tags for the feature as well as 2 other tags for the scenario. Tags always starts with a *@* followed by any text without spaces in it.

Phobo supports a few special tags that are reserved by Phobo. These tags will have an effect on the execution of the tests. These are:

- `@clearLocalStorage`: This tag can be added to any scenario. When a scenario has this tag it will automatically trigger a cleanup step that clears all local storage data for a browser page whenever the scenario has finished. It can be used to for example always clean up local storage after browsing a site.

- `@cleanupAwsResources`: This tag can be added to any scenario. When a scenario has this tag it will automatically trigger a cleanup step at the end of the scenario that deletes all AWS resources created by any actions in the scenario.
