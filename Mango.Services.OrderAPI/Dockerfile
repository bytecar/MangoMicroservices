# syntax=docker/dockerfile:1

# Define the .NET version to use
ARG DOTNET_VERSION=9.0

# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:${DOTNET_VERSION} AS builder
WORKDIR /source

# Copy the project file and restore dependencies
COPY ["./Mango.Services.OrderAPI/Mango.Services.OrderAPI.csproj", "./Mango.Services.OrderAPI"]
COPY ["./Mango.MessageBus/Mango.MessageBus.csproj", "./Mango.MessageBus"]

RUN --mount=type=cache,target=/root/.nuget/packages \
    --mount=type=cache,target=/root/.cache/msbuild \
    dotnet restore "./Mango.Services.OrderAPI/Mango.Services.OrderAPI.csproj"

# Copy the entire source code and build the application
COPY . .


WORKDIR "./Mango.Services.OrderAPI"
RUN dotnet build "Mango.Services.OrderAPI.csproj" -c Release -o /app/build

WORKDIR /source

RUN dotnet publish -c Release -o /app/publish

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:${DOTNET_VERSION} AS final
WORKDIR /app

# Copy the built application from the builder stage
COPY --from=builder /app/publish .

# Set the entry point for the application
ENTRYPOINT ["dotnet", "Mango.Services.OrderAPI.dll"]