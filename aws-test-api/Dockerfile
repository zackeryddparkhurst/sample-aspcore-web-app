#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["aws-test-api/aws-test-api.csproj", "aws-test-api/"]
RUN dotnet restore "aws-test-api/aws-test-api.csproj"
COPY . .
WORKDIR "/src/aws-test-api"
RUN dotnet build "aws-test-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "aws-test-api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "aws-test-api.dll"]