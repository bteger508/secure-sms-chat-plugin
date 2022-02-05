1. Change the SQL connection string in appsettings.json from "server=sql; ..." to "server=localhost,1433; ..."
2. In the root of the repository: `docker compose up --build`
    - If the build fails, run  `docker compose up --build` again
3. Make sure there are no previous migrations in the Migrations subfolder (SignalRChat/Migrations). 
    - If there is a Migrations folder, delete the folder. 
4. From visual studio, open the package manager
    - Add a new migration with `Add-Migration initial`
    - Update the docker MSSQL database with `Update-Database`
5. From Docker Desktop, launch SQL Pad in your browser:
    - username is ethanmhollan@gmail.com (yes, no 'd' in holland)
    - password is Password1!
    - Make sure the conversations and messages tables were added to the database
6. Change the SQL connection string in appsettings.json from "server=localhost,1433; ..." to "server=sql; ..."
7. Docker compose up from the root of the repo, and the API and Database should work fine. 
