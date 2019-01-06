#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["WebAppCore_Docker_cicd/WebAppCore_Docker_cicd.csproj", "WebAppCore_Docker_cicd/"]
RUN dotnet restore "WebAppCore_Docker_cicd/WebAppCore_Docker_cicd.csproj"
COPY . .
WORKDIR "/src/WebAppCore_Docker_cicd"
RUN dotnet build "WebAppCore_Docker_cicd.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WebAppCore_Docker_cicd.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebAppCore_Docker_cicd.dll"]