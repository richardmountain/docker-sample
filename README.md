# docker-sample
As the name suggests, in attempt to learn how to use docker with my dotnet projects, I have created a sample file of things to remember.

If you have any suggestions please by all means let me know.

1. Create `Dockerfile` in the root of your solution and copy/paste following block and edit accordingly.
```
# https://docs.docker.com/samples/dotnetcore/
# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app
    
# Copy csproj and restore as distinct layers
COPY . ./
RUN dotnet restore
    
# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out
    
# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "{Change this to your project}.dll"]
```

2. Run `docker build -t {docker-tag} .`
Change `{docker-tag}` to something more meaning full.
You can also check this is successful by running `docker images`, you should see it listed.

3. To run the project from docker `docker run -d -p 8080:80 --name {name-the-docker-container} {docker-tag}

You should then following this example be able to access your website via a browser `http://localhost:8080`
