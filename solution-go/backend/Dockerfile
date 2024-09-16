# Stage 1: Build the Go application
FROM golang:1.19 AS build

WORKDIR /app

# Copy the Go module files
COPY go.mod go.sum ./
RUN go mod download


# Copy the rest of the source code
COPY . .

# Build the application
RUN go build -o /employee-management-system

# Stage 2: Run the application
FROM golang:1.19

WORKDIR /app

# Copy the built binary from the build stage
COPY --from=build /employee-management-system /app/employee-management-system

# Copy the wait-for-it.sh script into the container
COPY wait-for-it.sh /app/wait-for-it.sh

# Make sure the wait-for-it.sh script is executable
RUN chmod +x /app/wait-for-it.sh



# Expose the backend port
EXPOSE 8080

# Set environment variables for the database connection (these will be overridden by Docker Compose)
ENV DB_HOST=localhost \
    DB_USER=postgres \
    DB_PASSWORD=password \
    DB_NAME=employee_db \
    DB_PORT=5432 \
    ALLOWED_ORIGINS=*

# Command to run the application
CMD ["/app/employee-management-system"]
