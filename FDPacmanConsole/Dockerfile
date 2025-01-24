FROM mcr.microsoft.com/dotnet/runtime:9.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FDPacmanConsole/FDPacmanConsole.csproj", "FDPacmanConsole/"]
RUN dotnet restore "FDPacmanConsole/FDPacmanConsole.csproj"
COPY . .
WORKDIR "/src/FDPacmanConsole"
RUN dotnet build "FDPacmanConsole.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "FDPacmanConsole.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FDPacmanConsole.dll"]
