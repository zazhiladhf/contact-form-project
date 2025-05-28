# Stage 1: Build the application
FROM golang:1.22-alpine AS builder

# Enable Go Modules
ENV GO111MODULE=on \
    CGO_ENABLED=0

# Set working directory
WORKDIR /app

# Copy the source code
COPY . .

# Download dependencies
RUN go mod download

# Build the application — let Docker/host handle the arch (GOARCH auto)
RUN go build -o api-contact-form .

# Stage 2: Create the production image
FROM alpine:latest

# Install tzdata for timezone support
RUN apk add --no-cache tzdata

# Set the timezone
ENV TZ=Asia/Jakarta
RUN ln -sf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Working directory
WORKDIR /app

# Create user and group
RUN addgroup -g 1001 binarygroup \
 && adduser -D -u 1001 -G binarygroup userapp

# Copy binary with permission set
COPY --from=builder --chown=userapp:binarygroup /app/api-contact-form .

# Switch to non-root user
USER userapp

# Expose port
EXPOSE 8080

# Run the binary
CMD ["./api-contact-form"]
