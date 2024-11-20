# Build stage: compiles and publishes the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy only the csproj and restore dependencies as a distinct layer
COPY ["source/PokeTracker/PokeTracker.csproj", "source/PokeTracker/"]
RUN dotnet restore "source/PokeTracker/PokeTracker.csproj"

# Copy the remaining files and build the application
COPY . .
WORKDIR "/src/source/PokeTracker"
RUN dotnet publish "PokeTracker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final stage: copies publish output from publish stage
FROM mcr.microsoft.com/dotnet/runtime:8.0 AS final
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "PokeTracker.dll"]
