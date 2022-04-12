# Deployment Documentation

## Initial Instructions
To deploy the code, you will need to install the following:
- [dotnet 6 sdk](https://docs.microsoft.com/en-us/dotnet/core/install/windows?tabs=net60)
- [Docker](https://www.docker.com/get-started)
- [NPM]()
- [Vue-cli]()

Clone the [bsu.secure-communications repository](https://bitbucket.org/accutechdev/bsu.secure-communications/src/master/)
onto the server you plan to use for sending and storing messages: `git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git`

## Adjusting Appsettings

Before you can deploy the application, you will need to replace the API keys stored in appsettings.json and appsettings.Development.json with your own keys. 

API Keys Needed:
1. Twilio
2. SendGrid

### Registering for a Twilio account

You can register for a twilio account at https://www.twilio.com/. When you create an account the website gives you an API Key and Account Sid. You will also need to purchase a phone number through twilio. The documentation for this on the website is excellent. In appsettings.json and appsettings.Development.json, replace the values in the "TwilioAccountDetails" object with your account Sid, API key, and twilio phone number. 

<how to register for a sendgrid key>
    
### Registering for a SendGrid Account

You can register for a SendGrid Account at [https://signup.sendgrid.com/](https://signup.sendgrid.com/). 
After Signing up, navigate to "Settings" -> "Sender Authentication". Then set up a Single Sender. This Email address will be the address that all emails are sent from. 
Go back to "Settings" then to "API Keys." Or alternatively to this [link](https://app.sendgrid.com/settings/api_keys). Name your API Key whatever you would like. Select "Restricted Access" for API Key Permissions and scroll down to "Mail Send" and move the slider over to full access. Then Create the key and copy it. 
In the code, in appsettings.json and appsettings.development.json, replace the values "API_KEY" and "EmailAddress" with your new API key and your single sender email address.


## Deploying with Docker
1. `cd` into the root of your repository and `docker compose up --build`
    - Chat Application: http://localhost:8080
    - API Swaggar Page: http://localhost:5001
    - SQL Pad: http://localhost:3000
2. Optional: From Docker Desktop, launch SQL Pad in your browser:
    - username is ethanmhollan@gmail.com (yes, no 'd' in holland)
    - password is Password1!
    - Make sure the conversations and messages tables were added to the database

### Troubleshooting
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

  
## Deploying Manually

### API Deployment

1. `cd` into bsu.secure-communications/SignalRChat and run `dotnet restore` to install packages
2. Download and install [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)  
    - Use the default installation settings 
    - Once the setup is complete, copy your connection string for the master database
    Ex: "Server=localhost;Database=master;Trusted_Connection=True;MultipleActiveResultSets=True;"
3. Optional: download SQL Management Studio to manage the API database. Configure a new connection with your connection string and credentials
4. Open the SignalRChat/appsettings.json file and replace the connection string in the file with your connection string. Save your changes.
5. From the bsu.secure-communications/SignalRChat directory, run `dotnet ef database update` 
    - If you get an error saying "Data in the root level is invalid" check that SignalRChat.csproj contains the following XML tag:  
  `<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.21">`. You can add this package with `dotnet add package Microsoft.EntityFrameworkCore.Tools`
    - Optional: verify in SQL Server Management Studio that the conversations, messages, tenants, users, and userconversations tables got added to the database.
6. To run the API, `cd` into bsu.secure-communications/SignalRChat and run the command `dotnet run`
    - API Swaggar Page: http://localhost:5001
7. To shut down the API, press CONTROL + C in your terminal (Works on Mac and Windows).  

### Chat Application Deployment

1. `cd` into bsu.secure-communications/BasicVue
2. `npm install` to download all dependencies
    - if error about SignalR, run
  
      -- 'npm install @microsoft/signalr'
  
    - and similarly with Vue-Cli
      
      -- npm install -g @vue/cli
     
3. `npm run serve` to run the web application: http://localhost:8080


### Troubleshooting 

#### API

Sometimes the API will fail due to a few common issues:

1. Caches not being cleared properly: `Data at the root level is invalid`. To fix this, `cd` into bsu.secure-communications/SignalRChat and run `dotnet clean` and `dotnet restore`
2. API cannot connect to SQL Server. Check to make sure that your connection string is correct, and that SQL Server is running.
    - For further information, consult the official documentation for [SQL Server](https://docs.microsoft.com/en-us/sql/?view=sql-server-ver15)
3. Issues with Database migrations. Sometimes migrations or database updates will fail. If this happens, do the following. 
    a. Delete the migrations directory in bsu.securecommunications/SignalRChat
    b. `cd` into bsu.securecommunications/SignalRChat and run `dotnet ef migrations add initial --msbuildprojectextensionspath obj/local` and `dotnet ef database update`.
    c. If you get an error saying "Data in the root level is invalid" check that SignalRChat.csproj contains the following XML tag:  
  `<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.21">`. You can add this package with `dotnet add package Microsoft.EntityFrameworkCore.Tools`

#### Chat Application
For the frontend, most errors will likely be package related. This can be fixed with either  
        -- `npm install`  
or if specific packages don't get installed properly, you will need to install them individually  
        -- `example: npm install axios`
if this problem persists, make sure you are in the 'BasicVue' directory where the 'package.json' is stored.
