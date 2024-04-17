# Build stage
FROM rust:1.77.1 AS build
WORKDIR /app
COPY . .

# Install stable Rust and set as default
RUN rustup install stable
RUN rustup default stable

# Install formatting and linting tools
RUN rustup component add rustfmt clippy

# Format, Lint, Test, Build
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
