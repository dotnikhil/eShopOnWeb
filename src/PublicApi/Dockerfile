FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /bsrc

COPY src/PublicApi/PublicApi.csproj .
RUN dotnet restore PublicApi.csproj

COPY . .
RUN dotnet build ./src/PublicApi/PublicApi.csproj -c Release -o ./build

FROM build AS publish
RUN dotnet publish ./src/PublicApi/PublicApi.csproj -c Release -o ./publish /p:UseAppHost=false

FROM base AS final
ENV ASPNETCORE_URLS=http://*:5000
WORKDIR /app
COPY --from=publish /bsrc/publish .
ENTRYPOINT ["dotnet", "PublicApi.dll"]



