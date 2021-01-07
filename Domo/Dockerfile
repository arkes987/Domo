#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["Domo/Domo.csproj", "Domo/"]
COPY ["Task/Domo.Task.Application/Domo.Task.Application.csproj", "Task/Domo.Task.Application"]
COPY ["Task/Domo.Task.Contract/Domo.Task.Contract.csproj", "Task/Domo.Task.Contract"]
COPY ["Task/Domo.Task.Domain/Domo.Task.Domain.csproj", "Task/Domo.Task.Domain"]
COPY ["Task/Domo.Task.Infrastructure/Domo.Task.Infrastructure.csproj", "Task/Domo.Task.Infrastructure"]
RUN dotnet restore "Domo/Domo.csproj"
COPY . .
WORKDIR "/src/Domo"
RUN dotnet build "Domo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Domo.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Domo.dll"]