
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
ENV ASPNETCORE_URLS=http://0.0.0.0:80
ENV ASPNETCORE_ENVIRONMENT=Production

# Atualizar pacotes e instalar FFmpeg
RUN apt-get update && \
    apt-get install -y ffmpeg

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore "ProcessadorVideo.Gerenciador/adapter/ProcessadorVideo.Api/ProcessadorVideo.Gerenciador.Api.csproj"
COPY . .
WORKDIR "/src/ProcessadorVideo.Gerenciador/adapter/ProcessadorVideo.Api"
RUN dotnet build "ProcessadorVideo.Gerenciador.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ProcessadorVideo.Gerenciador.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ProcessadorVideo.Gerenciador.Api.dll"]
