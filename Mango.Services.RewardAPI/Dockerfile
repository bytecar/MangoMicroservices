# syntax=docker/dockerfile:1

# Define the .NET version to use
ARG DOTNET_VERSION=9.0

# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:${DOTNET_VERSION} AS builder

# Set the working directory
WORKDIR /source

# Copy the project file and restore dependencies
COPY ["./Mango.Services.RewardAPI/Mango.Services.RewardAPI.csproj", "./Mango.Services.RewardAPI"]
RUN --mount=type=cache,target=/root/.nuget/packages \
    --mount=type=cache,target=/root/.cache/msbuild \
    dotnet restore "./Mango.Services.RewardAPI/Mango.Services.RewardAPI.csproj"

# Copy the rest of the application source code
COPY . .


WORKDIR "./Mango.Services.RewardAPI"
RUN dotnet build "Mango.Services.RewardAPI.csproj" -c Release -o /app/build

WORKDIR /source

# Build and publish the application
RUN dotnet publish -c Release -o /app/publish

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:${DOTNET_VERSION} AS final

# Set the working directory
WORKDIR /app

# Copy the published application from the builder stage
COPY --from=builder /app/publish .

# Expose the port the application runs on
EXPOSE 80

# Define the entry point for the container
ENTRYPOINT ["dotnet", "Mango.Services.RewardAPI.dll"]