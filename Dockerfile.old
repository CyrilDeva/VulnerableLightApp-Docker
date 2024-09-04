FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY VulnerableWebApplication.csproj .
RUN dotnet restore "VulnerableWebApplication.csproj"
COPY . .
RUN dotnet publish "VulnerableWebApplication.csproj" -c Release -o /publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /publish .

ENTRYPOINT ["dotnet", "VulnerableWebApplication.dll"]
