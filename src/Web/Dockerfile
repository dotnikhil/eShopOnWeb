FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /fsrc

COPY src/Web/Web.csproj .
RUN dotnet restore Web.csproj

COPY . .
RUN dotnet build ./src/Web/Web.csproj -c Release -o ./build

FROM build AS publish
RUN dotnet publish ./src/Web/Web.csproj -c Release -o ./publish /p:UseAppHost=false

FROM base AS final
ENV ASPNETCORE_URLS=http://*:80 
COPY --from=publish /fsrc/publish .
ENTRYPOINT ["dotnet", "Web.dll"]
