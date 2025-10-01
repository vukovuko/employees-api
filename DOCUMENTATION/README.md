# TheEmployeeAPI Documentation

## Overview

TheEmployeeAPI is a .NET 9.0 Web API project that provides employee management functionality. This is a minimal API implementation using ASP.NET Core's minimal hosting model with route groups for organized endpoint management.

## Project Structure

```
TheEmployeeAPI/
├── Program.cs                    # Main application entry point
├── Employee.cs                   # Employee data model
├── TheEmployeeAPI.csproj        # Project file with dependencies
├── appsettings.json             # Production configuration
├── appsettings.Development.json # Development configuration
├── Properties/
│   └── launchSettings.json      # Launch configuration
├── TheEmployeeAPI.http          # HTTP test file
├── TheEmployeeAPI.Tests/        # Unit test project
│   └── UnitTest1.cs            # Test file
└── DOCUMENTATION/               # This documentation folder
    ├── README.md               # This file
    ├── API-Reference.md        # API endpoint documentation
    ├── Configuration.md        # Configuration documentation
    ├── Setup-Guide.md          # Setup and deployment guide
    └── Architecture.md         # Architecture and design patterns
```

## Quick Start

1. **Prerequisites**: .NET 9.0 SDK
2. **Run the application**: `dotnet run`
3. **Access the API**: `https://localhost:7028` or `http://localhost:5039`
4. **Test endpoints**: `GET /employees`, `GET /employees/{id}`, `POST /employees`
5. **Run tests**: `dotnet test`

## Documentation Files

- [API Reference](API-Reference.md) - Complete API endpoint documentation
- [Configuration](Configuration.md) - Configuration files and settings
- [Setup Guide](Setup-Guide.md) - Installation and deployment instructions
- [Architecture](Architecture.md) - System architecture and design patterns

## Features

- **Employee Management API**: CRUD operations for employee data
- **Route Groups**: Organized endpoints using `/employees` route group
- **Unit Testing**: xUnit test project with `dotnet test` support
- **OpenAPI Support**: Swagger/OpenAPI documentation in development mode
- **HTTPS Redirection**: Automatic HTTPS redirection for security
- **Minimal API**: Clean, minimal API implementation without controllers
- **Data Validation**: Required properties with nullable reference types

## Technology Stack

- **Framework**: ASP.NET Core 9.0
- **Language**: C# 12
- **API Style**: Minimal APIs
- **Documentation**: OpenAPI/Swagger
- **Hosting**: Kestrel web server

## Development

The project uses the minimal hosting model introduced in .NET 6, which allows for streamlined API development without the traditional MVC controller structure. All endpoints are defined directly in the `Program.cs` file using route groups and the `MapGet`, `MapPost`, etc. methods. The project includes an `Employee` class with required properties and treats warnings as errors for better code quality.
