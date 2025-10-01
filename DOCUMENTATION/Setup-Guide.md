# Setup Guide

## Prerequisites

### Required Software

- **.NET 9.0 SDK** or later
- **Visual Studio 2022** (17.8+) or **Visual Studio Code** with C# extension
- **Git** (for version control)

### System Requirements

- **Operating System**: Windows 10/11, macOS 10.15+, or Linux
- **Memory**: Minimum 4GB RAM (8GB recommended)
- **Disk Space**: At least 1GB free space

## Installation

### 1. Install .NET 9.0 SDK

#### Windows
1. Download from [Microsoft .NET Downloads](https://dotnet.microsoft.com/download)
2. Run the installer
3. Verify installation: `dotnet --version`

#### macOS
```bash
# Using Homebrew
brew install --cask dotnet

# Or download from Microsoft website
```

#### Linux (Ubuntu/Debian)
```bash
# Add Microsoft package repository
wget https://packages.microsoft.com/config/ubuntu/24.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb

# Install .NET 9.0 SDK
sudo apt-get update
sudo apt-get install -y dotnet-sdk-9.0
```

### 2. Clone the Repository

```bash
git clone <repository-url>
cd TheEmployeeAPI
```

### 3. Restore Dependencies

```bash
dotnet restore
```

## Running the Application

### Development Mode

#### Using .NET CLI
```bash
# Run with HTTP only
dotnet run --launch-profile http

# Run with HTTPS
dotnet run --launch-profile https

# Run with default profile
dotnet run
```

#### Using Visual Studio
1. Open `TheEmployeeAPI.csproj` in Visual Studio
2. Press F5 or click "Start Debugging"
3. Select the desired launch profile

#### Using Visual Studio Code
1. Open the project folder
2. Press F5 or use Command Palette: "Debug: Start Debugging"
3. Select ".NET Core" configuration

### Production Mode

```bash
# Build for production
dotnet build --configuration Release

# Run in production mode
dotnet run --configuration Release --environment Production
```

## Configuration

### Environment Variables

Set the environment variable for different modes:

```bash
# Windows (PowerShell)
$env:ASPNETCORE_ENVIRONMENT="Development"

# Windows (Command Prompt)
set ASPNETCORE_ENVIRONMENT=Development

# macOS/Linux
export ASPNETCORE_ENVIRONMENT=Development
```

### Port Configuration

Default ports are configured in `Properties/launchSettings.json`:

- **HTTP**: 5039
- **HTTPS**: 7028

To change ports, modify the `applicationUrl` in the launch settings or use:

```bash
dotnet run --urls "http://localhost:5000;https://localhost:5001"
```

## Testing the Application

### 1. Using the HTTP File

The project includes `TheEmployeeAPI.http` for testing:

```http
@TheEmployeeAPI_HostAddress = http://localhost:5039

GET {{TheEmployeeAPI_HostAddress}}/weatherforecast/
Accept: application/json
```

### 2. Using curl

```bash
# Test weather forecast endpoint
curl -X GET "http://localhost:5039/weatherforecast" \
     -H "Accept: application/json"
```

### 3. Using a Web Browser

Navigate to:
- **HTTP**: http://localhost:5039/weatherforecast
- **HTTPS**: https://localhost:7028/weatherforecast

### 4. Using Postman

1. Create a new GET request
2. URL: `http://localhost:5039/weatherforecast`
3. Headers: `Accept: application/json`
4. Send the request

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Find process using port 5039
netstat -ano | findstr :5039

# Kill the process (Windows)
taskkill /PID <process_id> /F

# Find and kill process (macOS/Linux)
lsof -ti:5039 | xargs kill -9
```

#### .NET SDK Not Found
```bash
# Check if .NET is installed
dotnet --version

# If not found, reinstall .NET 9.0 SDK
```

#### SSL Certificate Issues
```bash
# Trust development certificate
dotnet dev-certs https --trust
```

#### Build Errors
```bash
# Clean and rebuild
dotnet clean
dotnet restore
dotnet build
```

### Logs and Debugging

#### Enable Detailed Logging
Add to `appsettings.Development.json`:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "Microsoft.AspNetCore": "Information"
    }
  }
}
```

#### View Logs
```bash
# Run with console logging
dotnet run --environment Development
```

## Deployment

### Docker Deployment

Create a `Dockerfile`:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY ["TheEmployeeAPI.csproj", "."]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TheEmployeeAPI.dll"]
```

Build and run:
```bash
docker build -t theemployeeapi .
docker run -p 8080:80 theemployeeapi
```

### IIS Deployment

1. Install ASP.NET Core Hosting Bundle
2. Publish the application:
   ```bash
   dotnet publish -c Release -o ./publish
   ```
3. Deploy to IIS following [Microsoft's guide](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/)

### Linux Deployment

1. Install .NET 9.0 Runtime
2. Publish the application:
   ```bash
   dotnet publish -c Release -o ./publish
   ```
3. Run with systemd or as a service

## Performance Considerations

### Development
- Use `dotnet watch` for hot reload during development
- Enable detailed logging for debugging

### Production
- Use Release configuration
- Configure proper logging levels
- Set up monitoring and health checks
- Consider using a reverse proxy (nginx, Apache)
