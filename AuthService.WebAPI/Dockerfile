#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["AuthService.WebAPI/AuthService.WebAPI.csproj", "AuthService.WebAPI/"]
COPY ["AuthService.Repository/AuthService.Repository.csproj", "AuthService.Repository/"]
COPY ["AuthService.DataAccess/AuthService.DataAccess.csproj", "AuthService.DataAccess/"]
COPY ["AuthService.BusinessObject/AuthService.BusinessObject.csproj", "AuthService.BusinessObject/"]
RUN dotnet restore "AuthService.WebAPI/AuthService.WebAPI.csproj"
COPY . .
WORKDIR "/src/AuthService.WebAPI"
RUN dotnet build "AuthService.WebAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AuthService.WebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AuthService.WebAPI.dll"]