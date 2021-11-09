# Deployment 
## Instructions
1. Clone the repository onto your computer or server
2. To run the project as a Docker container:  
  a. open docker desktop
  b. open the SignalRChat.sln solution file in Visual Studio (Windows version)  
  b. run the SignalRChat project in the debugger with the docker option  
3. To run the project outside of Docker  
  a. Open your terminal  
  b. cd into the root directory of the cloned repository  
  c. Use the command "dotnet run" to run the application  
  
## Troubleshooting
If the project does not run properly try the following:
1. Check to make sure that docker desktop is running properly
2. If you are running the application from the command line, check to make sure you are in the right directory
  - The root directory of the repository should contain the appsettings.json file
