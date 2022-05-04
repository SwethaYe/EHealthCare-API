#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["ProjectManagement.Api/E-healthcare.Api.csproj", "ProjectManagement.Api/"]
COPY ["ProjectManagement.Shared/E-healthcare.Shared.csproj", "ProjectManagement.Shared/"]
COPY ["ProjectManagement.Data.Implementation/E-healthcare.Data.csproj", "ProjectManagement.Data.Implementation/"]
COPY ["ProjectManagement.Entities/E-healthcare.Entities.csproj", "ProjectManagement.Entities/"]
RUN dotnet restore "ProjectManagement.Api/E-healthcare.Api.csproj"
COPY . .
WORKDIR "/src/ProjectManagement.Api"
RUN dotnet build "E-healthcare.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "E-healthcare.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "E-healthcare.Api.dll"]