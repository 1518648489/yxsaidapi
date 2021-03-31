#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["Pear.Web.Entry/Pear.Web.Entry.csproj", "Pear.Web.Entry/"]
COPY ["Pear.Web.Core/Pear.Web.Core.csproj", "Pear.Web.Core/"]
COPY ["Pear.Application/Pear.Application.csproj", "Pear.Application/"]
COPY ["Pear.Core/Pear.Core.csproj", "Pear.Core/"]
COPY ["Pear.Database.Migrations/Pear.Database.Migrations.csproj", "Pear.Database.Migrations/"]
COPY ["Pear.EntityFramework.Core/Pear.EntityFramework.Core.csproj", "Pear.EntityFramework.Core/"]
RUN dotnet restore "Pear.Web.Entry/Pear.Web.Entry.csproj"
COPY . .
WORKDIR "/src/Pear.Web.Entry"
RUN dotnet build "Pear.Web.Entry.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Pear.Web.Entry.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Pear.Web.Entry.dll"]