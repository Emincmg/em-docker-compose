# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /source

# Copy solution and project files first for better layer caching
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore --runtime linux-x64 --verbosity minimal

# Copy everything else and build
COPY aspnetapp/. ./aspnetapp/
WORKDIR /source/aspnetapp

# Publish with optimizations
RUN dotnet publish \
    -c Release \
    -o /app \
    -r linux-x64 \
    --self-contained false \
    --no-restore \
    /p:DebugType=None \
    /p:DebugSymbols=false \
    /p:EnableCompressionInSingleFile=true \
    /p:TrimUnusedDependencies=true \
    /p:InvariantGlobalization=true

# Final stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Create non-root user and set permissions
RUN groupadd -g 1000 appgroup && \
    useradd -u 1000 -g appgroup -d /app -s /bin/bash --no-create-home appuser && \
    chown -R appuser:appgroup /app

# Copy the published application
COPY --from=build --chown=appuser:appgroup /app .

# Security hardening
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl && \
    rm -rf /var/lib/apt/lists/* && \
    find /usr/share/dotnet -type f -perm /a+x -exec strip {} \; && \
    find /usr/share/dotnet -type f -name "*.pdb" -delete && \
    find /usr/share/dotnet -type f -name "*.xml" -delete

USER appuser

# Health check endpoint (ensure your app has this endpoint)
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD curl -f http://localhost/health || exit 1

# Entry point
ENTRYPOINT ["dotnet", "aspnetapp.dll"]