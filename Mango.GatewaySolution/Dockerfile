# syntax=docker/dockerfile:1

# Define the .NET version to use
ARG DOTNET_VERSION=9.0

# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:${DOTNET_VERSION} AS builder
WORKDIR /source

# Copy the project file and restore dependencies
COPY ["./Mango.GatewaySolution/Mango.GatewaySolution.csproj", "./Mango.GatewaySolution"]
RUN --mount=type=cache,target=/root/.nuget/packages \
    --mount=type=cache,target=/root/.cache/msbuild \
    dotnet restore "./Mango.GatewaySolution/Mango.GatewaySolution.csproj"

# Copy the entire source code and build the application
COPY . .

WORKDIR "./Mango.GatewaySolution"
RUN dotnet build "Mango.GatewaySolution.csproj" -c Release -o /app/build

WORKDIR /source

RUN dotnet publish -c Release -o /app/publish

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:${DOTNET_VERSION} AS final
WORKDIR /app

# Copy the built application from the builder stage
COPY --from=builder /app/publish .

# Set a non-root user for security
RUN useradd -m appuser
USER appuser

# Expose the port the application runs on
EXPOSE 80

# Define the entry point for the container
ENTRYPOINT ["dotnet", "Mango.GatewaySolution.dll"]