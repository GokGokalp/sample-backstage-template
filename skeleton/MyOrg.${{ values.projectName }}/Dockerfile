FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE ${{ values.port }}

ENV ASPNETCORE_URLS=http://+:${{ values.port }}

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["MyOrg.${{ values.projectName }}/MyOrg.${{ values.projectName }}.csproj", "MyOrg.${{ values.projectName }}/"]
RUN dotnet restore "MyOrg.${{ values.projectName }}/MyOrg.${{ values.projectName }}.csproj"
COPY . .
WORKDIR "/src/MyOrg.${{ values.projectName }}"
RUN dotnet build "MyOrg.${{ values.projectName }}.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "MyOrg.${{ values.projectName }}.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyOrg.${{ values.projectName }}.dll"]