# Use the official Microsoft .NET Framework 4.8 image with IIS
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2019 AS base
WORKDIR /inetpub/wwwroot
EXPOSE 80

# Use the SDK image for building the application
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2019 AS build
WORKDIR /src

# Copy the solution file and restore NuGet packages
COPY *.sln ./
COPY Homify/ ./Homify/
RUN nuget restore

# Build the application
RUN msbuild Homify\Homify.csproj /p:DeployOnBuild=true /p:WebPublishMethod=FileSystem /p:publishUrl=..\publish

# Final stage/image
FROM base AS final
WORKDIR /inetpub/wwwroot
COPY --from=build /src/publish .