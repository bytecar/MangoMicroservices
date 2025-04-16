# Running the Mango Project with Docker

## Prerequisites

- Ensure Docker and Docker Compose are installed on your system.
- Verify that the .NET SDK version 9.0 is available as specified in the Dockerfiles.

## Environment Variables

- No specific environment variables are required to run the services as per the provided Dockerfiles and Compose file.

## Build and Run Instructions

1. Clone the repository and navigate to the project root directory.
2. Use the following command to build and start the services:

   ```bash
   docker-compose up --build
   ```

3. Access the services via the exposed ports as listed below.

## Exposed Ports

- `mango_gateway`: 8080
- `mango_web`: 8081

## Notes

- The services are interconnected using a Docker bridge network named `mango_network`.
- Ensure the required dependencies and configurations are in place before running the services.

For further details, refer to the individual service directories and their respective Dockerfiles.