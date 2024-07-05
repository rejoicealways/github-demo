
The following are instructions for setting up a React Native Expo app which will use Laravel Restful API 
within a Docker environment.

I am opting to develop the React Front end with the Laravel Back End in the same Docker environment.
This is as opposed to developing in two separate environments and using a external docker network
to communicate between the setups

# Start with organization

Establish your folder structure. 
Under your project directory name
```
docker-compose/
    nginx/
        conf.d/
docker-image/
    reactExpoApp/
    laravelBackend/
src/
    reactExpoApp/
    laravelBackend/
```

# Install Prerequisite software
Install Node.JS on your local machine, which also installs npm (which is what is used for this project)
brew install node


# Set up the Expo project
cd into the directory where to create the new React Expo app.  In this example,
`src/reactExpoApp/`

Deprecated
`npm install -g expo-cli`  //This should not be needed now since the local version of the expo cli is installed with every project

Then create your new reactExpoApp in that directory
```
npx create-expo-app --template
```
Choose the default setting (blank)

Next, to get this up and running in the web, you need to add some dependencies
`npx expo install react-native-web react-dom @expo/metro-runtime`

---
If you are just using straight react-native, this following would be run and see the Dockerfile

run
```
npx react-native init reactnapp 
```
in the `./src/` directory


# Set up the Docker environment 

## Docker Setup for React Native

`/docker-image/reactExpoApp/Dockerfile`
```
FROM node:latest
WORKDIR /reactExpoApp
EXPOSE 8081

#This is needed to keep the container running. I run the npm run start and npm run web commands
#from within the container
CMD ["tail", "-f","/dev/null"]  
```

`/docker-compose.yml` 
```
version: '3.9'

services:
  react-expofrontend:
    container_name: react-expofrontend
    build: ./docker-image/reactExpoApp
    ports:
      - '8081:8081'
    volumes:
      - ./src/reactExpoApp:/reactExpoApp
```

## Useful Docker commands
```
docker-compose build
docker-compose up
docker ps

//Cleans up dangling images and containers.  Any running container and images used will be left untouched
docker-compose system prune

//Builds the image and doesn't draw from the cache
docker-compose build --no-cache 
```


# TODO to figure out Instructions

Instructions for running npm-watch
add
```
"watch": {
    "build": {
      "patterns": [
        "ui",
        "components",
        "api",
        "constants",
        "hooks",
        "pages",
        "utils",
        "wrappers"
      ],
    "extensions": "js,jsx"
    }
},
```
