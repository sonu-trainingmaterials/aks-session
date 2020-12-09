FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS publish
WORKDIR /src
COPY *.csproj ./
RUN dotnet restore 
COPY . .
RUN dotnet build -c Release -o /app/build
RUN dotnet publish -c Release -o /dist

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS final
WORKDIR /app
EXPOSE 80

ENV ConnectionStrings:DefaultConnection="Server=tcp:sqlserver-3159.database.windows.net,1433;Database=bankdb;User ID=labuser;Password=Password@123;Encrypt=true;Connection Timeout=30;"
ENV JwtIssuerOptions:Issuer="BstBankService"
ENV JwtIssuerOptions:Audience="http://localhost:5000/"

COPY --from=publish /dist .
ENTRYPOINT ["dotnet", "BSTBankService.dll"]