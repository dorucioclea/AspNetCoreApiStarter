FROM mcr.microsoft.com/dotnet/core/aspnet:2.1 AS base
WORKDIR /app
ENV ASPNETCORE_ENVIRONMENT=Production
ENV ASPNETCORE_URLS http://*:5000
EXPOSE 5000

FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS builder
ARG Configuration=Release
WORKDIR /src
COPY *.sln ./
COPY Web.Api/Web.Api.csproj Web.Api/
COPY Web.Api.Core/Web.Api.Core.csproj Web.Api.Core/
COPY Web.Api.Infrastructure/Web.Api.Infrastructure.csproj Web.Api.Infrastructure/
COPY Tests/Web.Api.Core.UnitTests/Web.Api.Core.UnitTests.csproj Tests/Web.Api.Core.UnitTests/
COPY Tests/Web.Api.Infrastructure.UnitTests/Web.Api.Infrastructure.UnitTests.csproj Tests/Web.Api.Infrastructure.UnitTests/
COPY Tests/Web.Api.IntegrationTests/Web.Api.IntegrationTests.csproj Tests/Web.Api.IntegrationTests/
COPY Tests/Web.Api.UnitTests/Web.Api.UnitTests.csproj Tests/Web.Api.UnitTests/

RUN dotnet restore
COPY . .
WORKDIR /src/Web.Api
RUN dotnet build -c $Configuration -o /app

FROM builder AS publish
ARG Configuration=Release
RUN dotnet publish -c $Configuration -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Web.Api.dll"]