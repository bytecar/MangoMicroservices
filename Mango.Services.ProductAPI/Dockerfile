# syntax=docker/dockerfile:1

# Define the .NET version to use
ARG DOTNET_VERSION=9.0

# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:${DOTNET_VERSION} AS builder

# Set the working directory
WORKDIR /source

# Copy the project file and restore dependencies
COPY ["./Mango.Services.ProductAPI/Mango.Services.ProductAPI.csproj", "./Mango.Services.ProductAPI"]
RUN --mount=type=cache,target=/root/.nuget/packages \
    --mount=type=cache,target=/root/.cache/msbuild \
    dotnet restore "./Mango.Services.ProductAPI/Mango.Services.ProductAPI.csproj"

# Copy the rest of the application source code
COPY . .


WORKDIR "./Mango.Services.ProductAPI"
RUN dotnet build "Mango.Services.ProductAPI.csproj" -c Release -o /app/build

WORKDIR /source

# Build the application
RUN dotnet publish -c Release -o /app/publish

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:${DOTNET_VERSION} AS final

# Set the working directory
WORKDIR /app

# Copy the published application from the builder stage
COPY --from=builder /app/publish .

# Expose the port the application runs on
EXPOSE 80

# Set the entry point for the application
ENTRYPOINT ["dotnet", "Mango.Services.ProductAPI.dll"]