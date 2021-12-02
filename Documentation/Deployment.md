# Deployment 
## Instructions
1. Clone the [bsu.secure-communications repository](https://bitbucket.org/accutechdev/bsu.secure-communications/src/master/)
onto the server you plan to use for sending and storing messages.
2. Download and install [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)  
  - Use the default installation settings 
  - Once the setup is complete, copy your connection string:  
    Ex: "Server=localhost;Database=master;Trusted_Connection=True;MultipleActiveResultSets=True;"
3. Open the SignalRChat/appsettings.json file and replace the connection string in the file with your connection string. 
4. Save your changes.
5. Run the project as a Docker container with the following steps:  
  a. Make sure [docker desktop](https://www.docker.com/products/docker-desktop) is open and running.  
  b. `cd` into the root directory of your local copy of the repository.  
  c. Use the command `docker-compose up` from this directory to run the application.  
  
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
