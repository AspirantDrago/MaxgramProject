FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS base
WORKDIR /app

RUN mkdir -p /data \
    && chown "$APP_UID:$APP_UID" /data

ENV ASPNETCORE_HTTP_PORTS=8080
ENV ConnectionStrings__Default="Data Source=/data/maxgram.db"

USER $APP_UID
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
WORKDIR /src

COPY ["Maxgram.Backend.csproj", "./"]
RUN dotnet restore "Maxgram.Backend.csproj"

COPY . .
RUN dotnet publish "Maxgram.Backend.csproj" \
    -c Release \
    -o /app/publish \
    --no-restore \
    -p:UseAppHost=false

FROM base AS final
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "Maxgram.Backend.dll"]
