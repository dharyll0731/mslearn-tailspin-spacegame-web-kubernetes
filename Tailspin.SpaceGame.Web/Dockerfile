# Base image with ASP.NET runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# SDK image for building the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy csproj and restore
COPY ["Tailspin.SpaceGame.LeaderboardContainer/Tailspin.SpaceGame.LeaderboardContainer.csproj", "Tailspin.SpaceGame.LeaderboardContainer/"]
RUN dotnet restore "Tailspin.SpaceGame.LeaderboardContainer/Tailspin.SpaceGame.LeaderboardContainer.csproj"

# Copy rest of the code
COPY . .

# Set working directory and build
WORKDIR "/src/Tailspin.SpaceGame.LeaderboardContainer"
RUN dotnet build "Tailspin.SpaceGame.LeaderboardContainer.csproj" -c Release -o /app/build

# Publish stage
FROM build AS publish
RUN dotnet publish "Tailspin.SpaceGame.LeaderboardContainer.csproj" -c Release -o /app/publish

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Tailspin.SpaceGame.LeaderboardContainer.dll"]
