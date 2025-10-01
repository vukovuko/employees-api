# Configuration

## Configuration Files

The application uses ASP.NET Core's configuration system with multiple configuration sources.

### appsettings.json

Main configuration file for production settings.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

**Configuration Sections:**

| Section | Description | Values |
|---------|-------------|--------|
| `Logging` | Logging configuration | Object |
| `Logging.LogLevel` | Log level settings | Object |
| `Logging.LogLevel.Default` | Default log level | "Information" |
| `Logging.LogLevel.Microsoft.AspNetCore` | ASP.NET Core log level | "Warning" |
| `AllowedHosts` | Allowed host headers | "*" (all hosts) |

### appsettings.Development.json

Development-specific configuration that overrides production settings.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

**Note**: Development configuration inherits from `appsettings.json` and only overrides specific values.

### TheEmployeeAPI.csproj

Main project file with dependencies and build configuration.

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net9.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="9.0.9" />
  </ItemGroup>
</Project>
```

**Project Properties:**

| Property | Value | Description |
|----------|-------|-------------|
| `TargetFramework` | net9.0 | .NET 9.0 target framework |
| `Nullable` | enable | Nullable reference types enabled |
| `ImplicitUsings` | enable | Implicit using statements enabled |
| `TreatWarningsAsErrors` | true | All warnings treated as errors |

**Dependencies:**

| Package | Version | Description |
|---------|---------|-------------|
| `Microsoft.AspNetCore.OpenApi` | 9.0.9 | OpenAPI/Swagger support |

## Launch Settings

### Properties/launchSettings.json

Defines how the application is launched in different environments.

```json
{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "http://localhost:5039",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:7028;http://localhost:5039",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

**Launch Profiles:**

| Profile | Description | URL | Environment |
|---------|-------------|-----|-------------|
| `http` | HTTP only | http://localhost:5039 | Development |
| `https` | HTTPS with HTTP fallback | https://localhost:7028;http://localhost:5039 | Development |

**Profile Properties:**

| Property | Description | Values |
|----------|-------------|--------|
| `commandName` | How to launch the app | "Project" |
| `dotnetRunMessages` | Show .NET run messages | true/false |
| `launchBrowser` | Open browser on start | true/false |
| `applicationUrl` | URLs to listen on | Semicolon-separated list |
| `environmentVariables` | Environment variables | Key-value pairs |

## Environment Variables

### ASPNETCORE_ENVIRONMENT

Controls the application environment:
- **Development**: Development mode with additional features
- **Staging**: Staging environment
- **Production**: Production environment

### Configuration Priority

Configuration values are loaded in the following order (later sources override earlier ones):

1. `appsettings.json`
2. `appsettings.{Environment}.json`
3. Environment variables
4. Command line arguments

## Logging Configuration

### Log Levels

Available log levels (in order of severity):
- **Trace**: 0 - Most detailed
- **Debug**: 1 - Debug information
- **Information**: 2 - General information
- **Warning**: 3 - Warning messages
- **Error**: 4 - Error messages
- **Critical**: 5 - Critical errors
- **None**: 6 - No logging

### Current Logging Setup

- **Default Level**: Information
- **ASP.NET Core Level**: Warning
- **Provider**: Console (default)

## Security Configuration

### AllowedHosts

The `AllowedHosts` setting controls which host headers are allowed:

- **"*"**: Allow all hosts (current setting)
- **"localhost"**: Only allow localhost
- **"example.com;*.example.com"**: Allow specific domains

### HTTPS Configuration

- **HTTPS Redirection**: Enabled in `Program.cs`
- **Default Ports**: 
  - HTTP: 5039
  - HTTPS: 7028

## Custom Configuration

To add custom configuration:

1. Add settings to `appsettings.json`:
```json
{
  "CustomSettings": {
    "ApiKey": "your-api-key",
    "MaxRetries": 3
  }
}
```

2. Create a configuration class:
```csharp
public class CustomSettings
{
    public string ApiKey { get; set; } = string.Empty;
    public int MaxRetries { get; set; }
}
```

3. Register in `Program.cs`:
```csharp
builder.Services.Configure<CustomSettings>(
    builder.Configuration.GetSection("CustomSettings"));
```

## Environment-Specific Overrides

Create environment-specific files:
- `appsettings.Staging.json` - Staging overrides
- `appsettings.Production.json` - Production overrides

These files will automatically be loaded based on the `ASPNETCORE_ENVIRONMENT` variable.
