#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["test-177-app/test-177-app.csproj", "test-177-app/"]
RUN dotnet restore "test-177-app/test-177-app.csproj"
COPY . .
WORKDIR "/src/test-177-app"
RUN dotnet build "test-177-app.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "test-177-app.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "test-177-app.dll"]