#
##add dotnet runtime image for running app
#From mcr.microsoft.com/dotnet/aspnet:10.0 as base
#Workdir /app
#Expose 8080
#
##add the dotnet sdk to build our app
#From mcr.microsoft.com/dotnet/sdk:10.0 as publish
#Workdir /src
#
##restore the pakages if something change it will add in the image
#copy "./dockertest.csproj" .
#Run dotnet restore "./dockertest.csproj"
#
## create publish to the src/app directory of container
#copy . .
#Run dotnet publish "./dockertest.csproj" -c release -o /src/app --no-restore
## publish cmd first restore , build and publish our app
#
##running the app
#From base as final
#Workdir /app
#copy --from=publish /src/app .
#Entrypoint ["dotnet","dockertest.dll]
#

# Add dotnet runtime image for running app
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080

# Add the dotnet sdk to build our app
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS publish
WORKDIR /src

# Restore the packages (cached if project file doesn't change)
COPY ["dockertest.csproj", "."]
RUN dotnet restore "./dockertest.csproj"

# Copy everything else and publish to the /src/app directory
COPY . .
RUN dotnet publish "./dockertest.csproj" -c Release -o /src/app --no-restore

# Running the app
FROM base AS final
WORKDIR /app
COPY --from=publish /src/app .
ENTRYPOINT ["dotnet", "dockertest.dll"]
  