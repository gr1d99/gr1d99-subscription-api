FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["gr1d99-subscription-api/gr1d99-subscription-api.csproj", "gr1d99-subscription-api/"]
RUN dotnet restore "gr1d99-subscription-api/gr1d99-subscription-api.csproj"
COPY . .
WORKDIR "/src/gr1d99-subscription-api"
RUN dotnet build "gr1d99-subscription-api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "gr1d99-subscription-api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "gr1d99-subscription-api.dll"]
