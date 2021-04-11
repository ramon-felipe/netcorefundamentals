#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
COPY ["src/OdeToFood/OdeToFood.csproj", "src/OdeToFood/"]
COPY ["src/OdeToFood.Data/OdeToFood.Data.csproj", "src/OdeToFood.Data/"]
COPY ["src/OdeToFood.Core/OdeToFood.Core.csproj", "src/OdeToFood.Core/"]
RUN dotnet restore "src/OdeToFood/OdeToFood.csproj"
COPY . .
WORKDIR "/src/src/OdeToFood"
RUN dotnet build "OdeToFood.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "OdeToFood.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OdeToFood.dll"]