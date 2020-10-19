#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
ENV ASPNETCORE_ENVIRONMENT "Production"
ENV ORCHARD_APP_DATA "/app/App_Data"

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["OrchardSkills.OrchardCore.OrchardCMS/OrchardSkills.OrchardCore.OrchardCMS.csproj", "OrchardSkills.OrchardCore.OrchardCMS/"]
COPY ["OrchardSkills.OrchardCore.SaaS/SaaS.csproj", "OrchardSkills.OrchardCore.SaaS/"]
# descomentar em caso de utilização de módulo preview.
#COPY ["NuGet.config", ""]
RUN dotnet restore "OrchardSkills.OrchardCore.OrchardCMS/OrchardSkills.OrchardCore.OrchardCMS.csproj"
COPY . .
WORKDIR "/src/OrchardSkills.OrchardCore.OrchardCMS"
RUN dotnet build "OrchardSkills.OrchardCore.OrchardCMS.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "OrchardSkills.OrchardCore.OrchardCMS.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OrchardSkills.OrchardCore.OrchardCMS.dll"]