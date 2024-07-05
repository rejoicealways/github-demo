# Docker Getting Started overview

## What is Docker?
[Docker Overview](https://docs.docker.com/get-started/overview/)
Docker provides the ability to package and run an application in a loosely isolated environment 
called a container.

## Installation (Mac OS)
https://docs.docker.com/get-docker/
- Download, Drag into the Applications directory from the Downloads. 
- Double-click in the Applications directory and follow default instructions 
- You will need to enter your computer's admin password 
- Delete the .dmg that is no longer needed after installation in the Applications directory


## Core files needed to start a docker project

**Dockerfile** - This is used to configure the container with specific software and environment variables \
**docker-compose.yml** - This defines the various containers that you need for your development environment\
**docker-compose/** - I use a subfolder with the various setup configuration files that might be needed for 
                    nginx of mysql\ e.g. **docker-compose/nginx/default.conf**, and **docker-compose/mysql/init.d**  

*Optional* \
.env

## Commands
docker-compose build \
docker-compose up \
docker-compose down 


Delete Containers and images to start fresh for docker project
