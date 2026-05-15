# Stage 1: Build
FROM golang:1.26-alpine AS builder

WORKDIR /app

# Copy everything first (go mod tidy needs source to resolve imports)
COPY . .
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o skillpulse .

# Stage 2: Run — use scratch for minimal image
FROM scratch

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app/skillpulse /skillpulse

EXPOSE 8080

ENTRYPOINT ["/skillpulse"]
