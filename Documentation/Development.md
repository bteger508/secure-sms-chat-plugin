# Iteration 1 Development Documentation
  -- may take some messing with config files to work
  
## Techstack
 make sure you have installed
  - Docker
  - Docker Desktop (optional if you are comfortable without the ui)
  - AspNetCore / .Net 5
  - node.js
  
  -- Visual Studio is preffered as an IDE on a Windows operating system, but is possible without. (further iterations will ensure an easier development environment)
 
## Directory and File Structure
  at this stage in development the frontend and backend are developed separately until Docker-Compose is implemented
  
  - terminal commands are run at root directory, and all Docker builds will need to point to the intended Dockerfile (one for SignalRChat API and other for babyui Vue.js)
  - for the API path should look something like './bsu.secure-communications' for directory and './bsu.secure-communications/SignalRChat/' for Dockerfile
  - for the frontend path should look like './bsu.secure-communications/babyUI-testFrontend' for the directory and './bsu.secure-communications/babyUI-testFrontend/testVue'4
    
    -- if you don't see files for testVue, you may need to checkout 'feature/frontend-babyui' branch

 ## Set up Backend API
      
clone project -> in terminal

     -- git clone git@bitbucket.org:accutechdev/bsu.secure-communications.git
     - (or download into empty file) 
 
Build Dockerfile 

     -- docker build -f (location of dockerfile from directory) -t (tag of image) . 
     -- docker build -f signalRChat/Dockerfile -t signalrchat/signalrchat .  
        
        
then run container
     -- docker run ("image")
     -- docker run signalrchat/signalrchat
        
- from Visual Studio, you may build and run by selecting Docker from the Solution Platform and press run
- potential errors likely reference a path error, make sure you are pointing to the correct file, calling from the correct directory, or type cases are correct
     
Once running a dummy UI should be available at http://localhost:80/
     
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
