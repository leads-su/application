# Application Package for Go Lang
This package provides easy interface for creating console applications.

# Why
Most applications have the same internal structure when it comes to creating them.  
This package solves the problem of writing the same code all over again.

## Package Features
### Commands Support
This package supports dynamic commands addition, as well as provides basic versioning command.

### Flags Support
You can create and assign custom application flags and use them later in your application

### Configuration Support
This package is able to load configuration from `environment` as well as from configuration files.
There are 4 options related to application configuration and they will be discussed below.

# Important Information
All processing should be handled inside commands, the only contents of your `main.go` should be this package initialization.

For example, if you have to process a single file, create new command, for example - `parse_command.go`, register it like `app.RegisterCommand(commands.ParseCommand)`.

In this case, all logic related to file parsing should be placed inside the `parse_command.go`

# Examples
## Creating Application Instance
In order to start using this package, you have to initialize application configuration in the `main.go` of your application.  
This can be achieved with the following piece of code:
```go
app := application.NewApplication(application.Options{})
```
Please refer to `DOCUMENTATION.md` to see the list of available options you can pass to the constructor.

## Supplying Application Configuration Information
`application.Options{}` has 4 options related to application configuration, 3 of them depended on each other, while the other one can be used as it is.

### File Based Configuration
To initialize file based configuration, you have to supply `application.Options{}` with the following options:
```go
application.Options{
    HasConfiguration:   true,
    ConfigurationPath:  "/etc/your_app.d",
    ConfigurationFile:  "main.yaml",
}
```
- `HasConfiguration` - tells package whether application has file based configuration
- `ConfigurationPath` - specifies absolute path to the folder containing application configuration
- `ConfigurationFile` - specifies name of the main configuration file

### Environment Based Configuration
To enable support for environment based configuration, the only parameter you need to pass to `application.Options` is `ConfigurationEnvPrefix`
```go
application.Options{
    ConfigurationEnvPrefix: "APPLICATION"
}
```
This parameter tells package that any environment variable that starts with `APPLICATION` should be treated as configuration variable for application.

### Registering Commands
For example, if you want to register bundeled `version` command, you can do it like this
```go
app.RegisterCommand(commands.VersionCommand)
```

### Starting Application
To actually start application, you have to call `Start()` method
```go
err := app.Start()
if err != nil {
	logger.Fatalf("main", "failed to initialize application - %s", err.Error())
}
```