# Development Documentation 
## Techstack
 make sure you have the following installed:
  - Docker
  - Docker Desktop (optional if you are comfortable without the ui)
  - AspNetCore / .Net 3 (We will migrate to .Net 6 soon)
  - [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (We use the development edition)
    - Make sure to save the connection string for the 'master' database
  - [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) (optional for testing and managing the database)
  - node.js
  - vue-cli
  
  -- Visual Studio 2022 is preferred as an IDE on a Windows operating system, but is possible without. (further iterations will ensure an easier development environment)
 
## Directory and File Structure
  at this stage in development the frontend and backend are developed separately until Docker-Compose is implemented
  
  - terminal commands are run at root directory, and all Docker builds will need to point to the intended Dockerfile (one for SignalRChat API and other for babyui Vue.js)
  - for the API path should look something like './bsu.secure-communications' for directory and './bsu.secure-communications/SignalRChat/' for Dockerfile
  - for the frontend path should look like './bsu.secure-communications/babyUI-testFrontend' for the directory and './bsu.secure-communications/babyUI-testFrontend/testVue'4
    
    -- if you don't see files for testVue, you may need to checkout 'feature/frontend-babyui' branch

 ## Set up Backend API
      
clone project in terminal:  

     -- git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git  
 
 To download the packages needed for the project  
      -- cd into <local repository>/SignalRChat   
      -- run the command `dotnet restore`   
 
### Configure SQL Server with Docker:
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
 
### Configure SQL Server Manually:  
- In SQL Server Management Studio, copy the connection string for the 'master' database   
  - Alternatively, you can create a new database and copy the connection string for it  
- In appsettings.json, replace the database connection string with the connection string for your database  
- from Visual Studio, go to the Tools tab of the toolbar. In the dropdown menu, go to Nuget Package Manager -> Package Manager Console.  
- Use the following commands in the package manager console to setup the database tables:  
  - Add-Migration initial-migration  
  - Update-Database  
- Check the build output to make sure the migration executed properly  
- If you get an error saying "Data in the root level is invalid" check that SignalRChat.csproj contains the following XML tag:  
  `<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.21">`   
- Optional: verify in SQL Server Management Studio that the messages and conversations table got added to the database.  
- potential errors likely reference a path error:  
  - make sure you are pointing to the correct file, calling from the correct directory, or type cases are correct
  - in Visual Studio another thing to check is the configuration of the Connected Services accessed either at the top of the project directory or 'Project' of the toolbar and select 'Manage connected services'
 
 ###
  
To run the API:  
      -- cd into <local repository>/SignalRChat   
      -- run the command `dotnet run`  
 
Once running a API skeleton wep page should be available at http://localhost:5000/

### Testing the Backend API  
1. Run the API with  
      -- cd into <local repository>/SignalRChat  
      -- run the command `dotnet run`  
2. Enter any name into the user and recipient input boxes and a message, but leave the conversation ID blank (the system will automatically generate one)  
3. Send the message into the chat  
4. In SQL Management Studio, expand Databases -> System Databases -> master (or the db you created) -> Tables.  
 - Right-click on the Messages table to verify that the message you sent on the web page got stored in the database. 
 - Locate and copy the conversation Id for the message you sent.  
5. From your web browser, enter the URL: http://localhost:5000/api/messages/{your_conversation_ID}
 - Ex: http://localhost:5000/api/messages/1
6. Verify that the server returns a JSON response with the data for the message you sent. 
 
## Set up Frontend Vue.jj
 
  - this makes use of an external dummy api for datahandling only on the frontend, styling, and other frontend only functions. If you know you don't need the backend, at this stage it is mroe efficient to work without it to limit potential api errors when you only want to work on the fontend
 
 Clone the respository 
 
      -- git clone https://MYenLinT@bitbucket.org/bteger508/bsu.securechat-frontend.git

 ### Developing only frontend
  
 In the project directory run 
 
        npm install
 
  this will install necessary packages
     -- if axios is undefined or throws an error you may need to do a separate install with 'NPM install axios'
 
then from 'testVue' directory run

        npm run serve
        
- from Visual Studio you can select 'Any CPU' in the Solution Platform and press run, or simply 'debug' on the menu bar, then select 'Start Debugging'
- potential errors likely involve node.js and node-Packages, or missing dependencies. Run in terminal:
  - npm install
  - npm install vue
        
 Once running UI should be available at http://localhost:8080/
 
 ### Developing with the backend
 
    --connection is established with the api. For developing with api calls and more dynamic data handling
 
 Make sure you check out the 'adding-signalR' branch
 
 In the project directory run 
 
       npm install
 
   again this will install necessary packages
      -- it is very likley 'SignalR' will not be installed with this command, so you will need to install it with 'npm install @microsoft/signalr'
 
 Make sure you have the api running, you can access the UI at http://localhost:8080/ or http://localhost:8080/chathub
 In the API the open port should be http://localhost:5001 + '/chathub' and on the fontend it will correspondingly be http://localhost:5001/chathub
 
 The api SignalR Hub is currently accessed in 'Datahandler.js' with AccessHub. If you hever have an permissions or client ID error, make sure the 'HubConnectionBuilder()' is being called.
 There is a console log for successful connection to the API.
 
 
 Happy Coding!
