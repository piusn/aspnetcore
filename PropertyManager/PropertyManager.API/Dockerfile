#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["PropertyManager.API/PropertyManager.API.csproj", "PropertyManager.API/"]
RUN dotnet restore "PropertyManager.API/PropertyManager.API.csproj"
COPY . .
WORKDIR "/src/PropertyManager.API"
RUN dotnet build "PropertyManager.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PropertyManager.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PropertyManager.API.dll"]