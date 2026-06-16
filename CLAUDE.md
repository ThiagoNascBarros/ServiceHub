# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Build entire solution
dotnet build ServiceHub.sln

# Run the API (Development mode, opens Swagger at http://localhost:5062/swagger)
dotnet run --project src/ServiceHub.Api/ServiceHub.Api.csproj

# Run with HTTPS profile
dotnet run --project src/ServiceHub.Api/ServiceHub.Api.csproj --launch-profile https

# Restore packages
dotnet restore ServiceHub.sln

# Clean build artifacts
dotnet clean ServiceHub.sln
```

No test projects exist yet. When added, run them with:
```bash
dotnet test ServiceHub.sln
# Single test by name
dotnet test --filter "FullyQualifiedName~TestName"
```

## Architecture

This is a **.NET 8 Clean Architecture** solution targeting DDD. Dependency flow is strictly inward:

```
Api → Application → Domain
Api → Infrastructure → Domain
```

| Project | Role |
|---|---|
| `ServiceHub.Domain` | Entities, value objects, domain events, repository interfaces, domain services |
| `ServiceHub.Application` | Use cases / application services, DTOs, command/query handlers (CQRS if adopted) |
| `ServiceHub.Infrastructure` | EF Core DbContext, repository implementations, external service clients |
| `ServiceHub.Api` | ASP.NET Core controllers, middleware, DI wiring, Swagger/OpenAPI |

**Current state:** Domain, Application, and Infrastructure projects are scaffolded but empty (placeholder `Class1.cs` files). The API has only the default WeatherForecast template — the intent is to build real features into this structure.

## Key Conventions

- **Nullable reference types** are enabled across all projects — use `?` annotations and null-checks at boundaries.
- **Implicit usings** are enabled — no need to add `using System;` etc. manually.
- Controllers live in `src/ServiceHub.Api/Controllers/` and use `[ApiController]` + `[Route("[controller]")]`.
- Swagger UI is available at `/swagger` in Development; it is the default launch URL.
- `appsettings.Development.json` overrides `appsettings.json` locally — keep secrets out of both and use User Secrets (`dotnet user-secrets`) instead.
