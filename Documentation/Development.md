# Development Documentation 
## Techstack
 make sure you have the following installed:
  - Docker
  - Docker Desktop (optional if you are comfortable without the ui)
  - AspNetCore / .Net 3
  - [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (We use the development edition)
    - Make sure to save the connection string for the 'master' database
  - [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) (optional for testing and managing the database)
  - node.js
  - vue-cli
 
## Directory and File Structure
The three main modules at the top level of the project are `BasicVue`, `SignalRChat`, and `SignalRChat.Tests`:
1. `BasicVue` contains our Vue.js frontend
2. `SignalRChat` contains the API
3. `SignalRChat.Tests` contains unit tests for the API

## Setup with Docker (Preferred)
1. Clone project in the terminal: `git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git`
2. **To build and run the docker container, `cd` into the root of the repo and run `docker compose up --build`**
    - user interface: `localhost:8080`
    - sqlpad: `localhost:3000` (username: ethanmhollan, password: Password1!)
4. Seed the database the first time you build the application (For future builds, just run `docker compose up --build`)


### Seeding the Database
1. Make sure the docker container is running (`docker compose up --build`)
2. Change the SQL connection string in appsettings.json from "server=sql; ..." to "server=localhost,1433; ..."
3. `cd` into the SignalRChat subfolder and run `dotnet ef migrations add initial --msbuildprojectextensionspath obj/local`
5. Run `dotnet ef database update`
6. From Docker Desktop, launch SQL Pad in your browser:
    - username is ethanmhollan@gmail.com (yes, no 'd' in holland)
    - password is Password1!
    - Make sure the conversations, messages, users, userconversations, and tenant tables were added to the database
7. Copy the contents of the `SQL/seed.sql` script into SQL Pad and execute the query. This will seed the database.
8. Change the SQL connection string in appsettings.json from "server=localhost,1433; ..." to "server=sql; ..." and save your changes
9. Rebuild the container: `docker compose up --build` from the root of the repo. 


## Manual Setup
1. Clone project in the terminal: `git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git`
2. `cd` into /Signalrchat directory and run `dotnet restore`

### Configure SQL Server Manually:  
- In SQL Server Management Studio, copy the connection string for the 'master' database   
  - Alternatively, you can create a new database and copy the connection string for it  
- In appsettings.json, replace the database connection string with the connection string for your database  
- from Visual Studio, go to the Tools tab of the toolbar. In the dropdown menu, go to Nuget Package Manager -> Package Manager Console.  
- Use the following commands in the package manager console to setup the database tables:  
  - `Add-Migration initial-migration` 
  - `Update-Database` 
- If you get an error saying "Data in the root level is invalid" check that SignalRChat.csproj contains the following XML tag:  
  `<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.21">`   
- Optional: verify in SQL Server Management Studio that the conversations, messages, users, userconversations, and tenant tables got added to the database.  
- potential errors likely reference a path error:  
  - make sure you are pointing to the correct file, calling from the correct directory, or type cases are correct
  - in Visual Studio another thing to check is the configuration of the Connected Services accessed either at the top of the project directory or 'Project' of the toolbar and select 'Manage connected services'
 
 ### Running the API
- `cd` into /SignalRChat   
- `dotnet run`
 
### Set up the Frontend
1. Make sure the API is running
2. `cd` into `/BasicVue` and run `npm install`
    - if axios is undefined or throws an error you may need to do a separate install with 'NPM install axios'
2. To run the frontend: `npm run serve`
    - from Visual Studio you can select 'Any CPU' in the Solution Platform and press run, or simply 'debug' on the menu bar, then select 'Start Debugging'
    - potential errors likely involve node.js and node-Packages, or missing dependencies. Try re-running `npm install` and `npm install vue`
3. In the web browser's development console, check for the the `SignalR Connected` message. This indicates that the frontend successfully connected to the SignalR Hub. 
        
Once running UI should be available at http://localhost:8080/

## Testing

Secure sms chat plugin uses xunit for unit testing the API and cypress for testing the GUI. To run cypress tests, open the repo in your terminal:

    cd BasicVue
    npm run cypress:open

To run all xunit tests for the API, make sure you are in the root of the repository, and run the following:
    dotnet test


Happy Coding!
