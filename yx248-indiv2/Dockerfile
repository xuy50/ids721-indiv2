# Build stage
FROM rust:1.67 AS build
WORKDIR /app
COPY . .
# Install rustfmt
RUN rustup component add rustfmt
# Install clippy
RUN rustup component add clippy
RUN make format
RUN make lint
# Run tests
RUN make test
# Build application
RUN make build
# Run service
RUN make run

# Distroless runtime stage
FROM gcr.io/distroless/cc-debian11
COPY --from=build /app/target/release/yx248-indiv2 /app/

# Use non-root user
USER nonroot:nonroot

# Set up app directory
ENV APP_HOME=/app
WORKDIR $APP_HOME

# Expose port
EXPOSE 3000

# Run app
ENTRYPOINT ["/app/yx248-indiv2"]
