#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 9003

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Services/Contact.API/Contact.API/Contact.API.csproj", "Services/Contact.API/Contact.API/"]
COPY ["Services/Contact.API/Contact.API.Infrastructure/Contact.API.Infrastructure.csproj", "Services/Contact.API/Contact.API.Infrastructure/"]
COPY ["Services/Contact.API/Contact.API.Models/Contact.API.Models.csproj", "Services/Contact.API/Contact.API.Models/"]
COPY ["Services/Contact.API/Contact.API.Service/Contact.API.Service.csproj", "Services/Contact.API/Contact.API.Service/"]
RUN dotnet restore "Services/Contact.API/Contact.API/Contact.API.csproj"
COPY . .
WORKDIR "/src/Services/Contact.API/Contact.API"
RUN dotnet build "Contact.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Contact.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Contact.API.dll"]