# ITEC1460-Module11Lab
Deploying SQL Server on Linux

## Pull and Run SQL Server
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=YourStrongP@ssw0rd" -e "MSSQL_PID=Developer" -p 1433:1433 --name sqlserver --health-cmd '/opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P "YourStrongP@ssw0rd" -Q "SELECT 1" || exit 1' --health-interval 10s --health-timeout 3s --health-retries 10 --health-start-period 10s -d mcr.microsoft.com/mssql/server:2022-latest

understand this command:
Creates and starts a new container for SQL Server.
Sets up necessary passwords and configurations..
Maps port 1433 for database connections
Includes health checks to ensure SQL Server is running properly.

## Install SQL Server Command-Line Tools
- Add Microsoft repository to your environment
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list

- Import GPG key
GPG (GNU Privacy Guard) is an encryption software that helps secure digital communications. Microsoft, like many software vendors, signs their Linux packages with a GPG key. 
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

- Update the package list
sudo apt-get update

- Install Microsoft SQL tools (These tools allow you to run SQL statements on your SQL Server.)
sudo ACCEPT_EULA=Y apt-get install -y mssql-tools unixodbc-dev

- Add the SQL tools to the PATH variable 
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc source ~/.bashrc

## Verify Installation
- Test the connection
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'YourStrongP@ssw0rd' -Q "SELECT @@VERSION"

## Create a Test Database
- Create a new database
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'YourStrongP@ssw0rd' -Q "CREATE DATABASE TestDB"

- Verify the database was created
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'YourStrongP@ssw0rd' -Q "SELECT name FROM sys.databases"

## Managing Your SQL Server Container
- Stop SQL Server
docker stop sqlserver

- Start SQL Server
docker start sqlserver

- View container logs
docker logs sqlserver

- Check container status
docker ps -a

- Check if container exists and is stopped
docker ps -a | grep sqlserver

- Verify SQL Server is running
docker ps | grep sqlserver

## Clean Up
- Stop the container
docker stop sqlserver

## Part2
Build Your Pizza Delivery Tracking System