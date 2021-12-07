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
  
To configure the database:  
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
  
To run the API:  
      -- cd into <local repository>/SignalRChat   
      -- run the command `dotnet run`  
 
Once running a API skeleton wep page should be available at http://localhost:5000/
     
## Set up Frontend Vue.js
dubbed 'babyui' as it is the foundation for plug and play uis
    
checkout and fetch feature branch

        git checkout -t feature/frontend-babyui
 
then run

        npm run serve
        
- from Visual Studio select 'Any CPU' in the Solution Platform and press run, or simply 'debug' on the menu bar, then select 'Start Debugging'
- potential errors likely involve node.js and node-Packages, or missing dependencies. Run in terminal:
  - npm install
  - npm install vue
        
 Once running UI should be available at http://localhost:8080/
 
 
 Happy Coding!
