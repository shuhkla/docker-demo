########################## Stage 1 - ANGULAR ############################
FROM node:10 AS angular-build
WORKDIR /app

# install angular cli
RUN npm install -g @angular/cli

# install client side dependencies
COPY ClientApp/package.json ./ClientApp/
COPY ClientApp/package-lock.json ./ClientApp/
WORKDIR /app/ClientApp
RUN npm install

# copy over the angular source
WORKDIR /app
COPY ClientApp/. ./ClientApp/

# run angular prod build
WORKDIR /app/ClientApp
RUN ng build --prod

########################## Stage 2 - DOTNET CORE ############################
FROM microsoft/dotnet:2.2-sdk AS aspnetcore-build
# copy csproj and restore as distinct layers
#COPY *.csproj ./
#RUN dotnet restore --no-cache

# copy and publish app
COPY *.csproj ./
COPY *.cs ./
COPY Controllers/. ./Controllers/
COPY Pages/. ./Pages/

# publish web application
RUN dotnet publish -c Release -o out

########################## Application Launch ############################
FROM microsoft/dotnet:2.2-aspnetcore-runtime AS runtime
WORKDIR /app
# Copy built binaries from Stage 1 & 2
COPY --from=angular-build /app/ClientApp/dist ./ClientApp/dist
COPY --from=aspnetcore-build /app/out ./

ENTRYPOINT ["dotnet", "docker-angular.dll"]