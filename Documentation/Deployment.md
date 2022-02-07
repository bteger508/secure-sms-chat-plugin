# Deployment Documentation
## Setup the Backend/API
To setup the API, you will need to install the [dotnet 6 sdk](https://docs.microsoft.com/en-us/dotnet/core/install/windows?tabs=net60) and [Docker](https://www.docker.com/get-started)
1. Clone the [bsu.secure-communications repository](https://bitbucket.org/accutechdev/bsu.secure-communications/src/master/)
onto the server you plan to use for sending and storing messages.
  - `git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git`  
2. To download the packages needed for the project  
      -- cd into <local repository>/SignalRChat   
      -- run the command `dotnet restore`

### Configure SQL server with Docker:
  [[Add instructions here]]
  
### Configure SQL server manually:
1. Download and install [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)  
  - Use the default installation settings 
  - Once the setup is complete, copy your connection string for the master database
    Ex: "Server=localhost;Database=master;Trusted_Connection=True;MultipleActiveResultSets=True;"
2. Open the SignalRChat/appsettings.json file and replace the connection string in the file with your connection string. 
3. Save your changes.
4. from Visual Studio, go to the Tools tab of the toolbar. In the dropdown menu, go to Nuget Package Manager -> Package Manager Console.  
5. Use the following commands in the package manager console to setup the database tables:  
  - Add-Migration initial-migration  
  - Update-Database  
6. Check the build output to make sure the migration executed properly  
  - If you get an error saying "Data in the root level is invalid" check that SignalRChat.csproj contains the following XML tag:  
  `<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.21">`   
  - Optional: verify in SQL Server Management Studio that the messages and conversations table got added to the database.  
  - potential errors likely reference a path error:  
  - make sure you are pointing to the correct file, calling from the correct directory, or type cases are correct  
7. To run the API:
     -- cd into <your local repo>/SignalRChat  
     -- run the command `dotnet run`  
8. To shut down the API, press CONTROL + C in your terminal (Works on Mac and Windows).  
      



  
To run the API:  
      -- cd into <local repository>/SignalRChat   
      -- run the command `dotnet run`  
 
Once running a API skeleton wep page should be available at http://localhost:5000/
## Troubleshooting
If the project does not run properly first try the following:
1. Check to make sure that docker desktop is running properly
2. If you are running the application from the command line, check to make sure you are in the root directory of the repository. 
  - The directory should contain the docker-compose.yml file. 

When you run docker-compose up for the first time, you may get the following error:  

        "System.InvalidOperationException: Unable to configure HTTPS endpoint. No server certificate was specified, and the default developer certificate could not be found or is out of date."
If this occurs, run the following commands in the command line:
- `dotnet dev-certs https --clean`
- `dotnet dev-certs https -t`

If the console shows an error saying that Docker cannot mount files from a particular directory:
1. Go to Docker desktop and open settings. 
2. Go to the Resources tab
3. Add the path to the resources Docker needs. The error message should specify the path to the directory it needs. 
  
  
## Setup Frontend
  
  Make sure you have NPM and Vue-cli
  
1. Clone the [bsu.securechat-frontend](https://bitbucket.org/bteger508/bsu.securechat-frontend/src/adding-SignalR/)
onto the server you plan to house the website.
  - `git clone https://MYenLinT@bitbucket.org/bteger508/bsu.securechat-frontend.git`  
  
2. To download the packages needed for the project  
      -- cd into <local repository>/testVue   
      -- run the command `npm install`
  
    -if error about SignalR, run
  
      -- 'npm install @microsoft/signalr'
  
   - and similarly with Vue-Cli
      
       -- npm install -g @vue/cli
 
3. Start with 
  -npm run serve 
  
Configure with Backend
1. First check the endpoints specified in './Startup.cs'  
    a. in ConfigureServices(), AddCors => AddPolicy => WithOrigins('url') specifies the accessible point and should match the port or server the frontend is running on.   Similarly the variables underneath can configure the permissions further. Default at http://localhost:8080/  
    b. at the end of Configure(), UseEndpoints configures the backends accessible endpoints. {controller=Home} ist he port of server, and the mapped points are below, with '/chathub'.  Check to make sure your connection point matches, at Datahandler.js .withUrl("https://localhost:5001/chathub/" example: .withUrl("https://localhost:(port backend is running on)/chathub/"... example, backend is running on 5001 then ".withUrl("https://localhost:5001/chathub/"  
  
2. start frontend with  
        --'npm run serve'  
  
  It will be found at http://localhost:8080/ and http://localhost:8080/chathub  
  
## Troubleshooting  
  
  most errors will likely be package related. This can be fixed with either  
        --npm install  
  or if specific packages still arent available even after you will need to do it individually  
        -- example: npm install axios  
  
  if this problem persists, make sure you are in the 'testVue' directory or where 'package.json' is stored  
  
Happy Coding!!  

