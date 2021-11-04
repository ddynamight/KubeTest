#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["KubeTest.Services.Vehicle/KubeTest.Services.Vehicle.csproj", "KubeTest.Services.Vehicle/"]
RUN dotnet restore "KubeTest.Services.Vehicle/KubeTest.Services.Vehicle.csproj"
COPY . .
WORKDIR "/src/KubeTest.Services.Vehicle"
RUN dotnet build "KubeTest.Services.Vehicle.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "KubeTest.Services.Vehicle.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "KubeTest.Services.Vehicle.dll"]