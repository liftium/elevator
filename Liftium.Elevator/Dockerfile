﻿FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY Liftium.Elevator/Liftium.Elevator.csproj Liftium.Elevator/
WORKDIR /src/Liftium.Elevator
RUN dotnet restore "Liftium.Elevator.csproj"

COPY . .
WORKDIR "/src/Liftium.Elevator"
RUN dotnet build "Liftium.Elevator.csproj" -c $BUILD_CONFIGURATION -o /app/build /p:UseAppHost=false

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Liftium.Elevator.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Liftium.Elevator.dll"]
