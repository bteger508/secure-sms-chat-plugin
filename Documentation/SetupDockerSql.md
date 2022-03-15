# Use this Document for Reference if the Seeding Script Fails on Build

To setup the API, you will need to install the [dotnet 6 sdk](https://docs.microsoft.com/en-us/dotnet/core/install/windows?tabs=net60) and [Docker](https://www.docker.com/get-started)
1. Make sure there are no previous migrations in the Migrations subfolder (SignalRChat/Migrations). 
    - If there is a Migrations folder, delete the folder. 
2. In Powershell, navigate to the SignalRChat folder, and run `dotnet clean` and `dotnet restore`
3. Change the SQL connection string in appsettings.json from "server=sql; ..." to "server=localhost,1433; ..."
4. From visual studio, open the package manager
    - Add a new migration with `Add-Migration initial`
5. In Powershell, In the root of the repository: `docker compose up --build`
    - If the build fails, run  `docker compose up --build` again
6. From visual studio, open the package manager
    - Update the docker MSSQL database with `Update-Database`
7. From Docker Desktop, launch SQL Pad in your browser:
    - username is ethanmhollan@gmail.com (yes, no 'd' in holland)
    - password is Password1!
    - Make sure the conversations and messages tables were added to the database
8. Close all running containers
9. Change the SQL connection string in appsettings.json from "server=localhost,1433; ..." to "server=sql; ..."
10.  In powershell, In the root of the repository: `docker compose up --build`
    - If the build fails, run  `docker compose up --build` again
