#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:5.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["GopherServer/GopherServer.csproj", "GopherServer/"]
COPY ["GopherServer.Core/GopherServer.Core.csproj", "GopherServer.Core/"]
COPY ["GopherServer.Providers.FileProvider/GopherServer.Providers.FileProvider.csproj", "GopherServer.Providers.FileProvider/"]
COPY ["GopherServer.Providers.MacintoshGarden/GopherServer.Providers.MacintoshGarden.csproj", "GopherServer.Providers.MacintoshGarden/"]
COPY ["GopherServer.Providers.Rss/GopherServer.Providers.Rss.csproj", "GopherServer.Providers.Rss/"]
COPY ["GopherServer.Providers.WpJson/GopherServer.Providers.WpJson.csproj", "GopherServer.Providers.WpJson/"]
RUN dotnet restore "GopherServer/GopherServer.csproj"
COPY . .
WORKDIR "/src/GopherServer"
RUN dotnet build "GopherServer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GopherServer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GopherServer.dll"]
