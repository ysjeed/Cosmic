# Introduction
This is a personal fork of Cosmic server emulator for Global MapleStory (GMS) version 83.
The original repository can be found here: https://github.com/P0nk/Cosmic
This fork is with some QOL administration features on the docker deployment.
This fork is mainly for deployment using Docker to containerise the deployment.
It keeps things simple and reduces "it works on my end" type scenarios.
This is compatible to run on linux systems as well as windows systems, made possible by Docker.

# Requirements
All requirements are installed on the host
1. Git - to clone this repository
1. Docker - be it just docker desktop or just docker daemon only

# Getting started
The docker-compose will be setting the following containers:
- Database (maplestory-db) - the database is used by the server to store game data such as accounts, characters and inventory items.
- Server (maplestory-server) - the server is the "brain" and routes network traffic between the clients.
- Web administration (adminer-container) - myphp webadmin to edit the database via webpage instead of having administration tools installed on host

# Before starting using docker-compose
There are a few settings that needs to be set up before launching the containers via docker-compose.
There will be a lot of **CHANGE_ME** notation, these are fields recommended to be changed for your specific configuration. 
It is still best to read through to understand if you want or need to change them first. 

# docker-compose file

### Main changes for all services
1. container_name - added, to name the container more name friendly, **may cause issues when launching multiple instances due to name clash**
1. restart - added, set to unless-stopped, for easy restarting and auto starting
1. networks - added, an isolated internal bridged network for communication amongst these docker containers

## db service changes
1. MYSQL_USER - added, **CHANGE_ME**, adding a new user to the mysql db, this is the username to access the web admin apart from root, **only used during FIRST setup of db**
1. MYSQL_PASSWORD - added, **CHANGE_ME**, the password for the above new user, this is the password to access the web admin, **only used during FIRST setup of db**
1. MYSQL_ROOT_PASSWORD - edited, **CHANGE_ME**, for security purposes
1. MYSQL_ALLOW_EMPTY_PASSWORD - edited, **CHANGE_ME**, for security purposes
1. lower_case_table_names - added, to enforce portability across platforms, 1 sets the database to case-insensitivity, 0 sets to case-sensitive

## adminer service
1. ports - **CHANGE_ME**, left is the port you want to access on your host (change this only), right is the port that adminer uses (do not change this)

# config.yaml file
This is the file where you will edit to affect server behaviour.
The fields are pretty self explainatory by the original authors, I mainly formatted the comments due to partial OCD.
**CHANGE_ME** rules still applies, especially for server settings. I will elaborate about the notable ones.

1. DB_PASS -  **CHANGE_ME** to the same as docker-compose file MYSQL_ROOT_PASSWORD
1. HOST - **CHANGE_ME** WAN IPv4 address, 127.0.0.1 for localhost play, internal IP (check router) for local network play
1. LANHOST - **CHANGE_ME** WAN IPv4 address, 127.0.0.1 for localhost play, internal IP (check router) for local network play 
1. ENABLE_PIC - I switched it to false as it was annoying during login, **CHANGE_ME** if you want
1. ENABLE_PIN - I switched it to false as it was annoying during login, **CHANGE_ME** if you want

# Starting the server

1. Go to the folder holding all these files on your host
1. Enter command on the terminal:
   ```# docker-compose up --detach```
1. Access webadmin via a web browser on your host
   ```<ip to hosting server/localhost if locally hosted>:<port>```
   Example:
   ```127.0.0.1:8080```
1. Input credentials to access web admin:
   ```
   server: db
   username: db_admin <change accordingly to your MYSQL_USER>
   password: db_password <change accordingly to your MYSQL_PASSWORD>
   database: cosmic
   ```
   
# Stopping the server
1. Go to the folder holding all these files on your host
1. Enter command on the terminal:
   ```# docker-compose down```

# Game client
Please refer to the original repository on getting the source files: [Link](https://github.com/P0nk/Cosmic?tab=readme-ov-file#3---client)
I recommend using ImHex, a hex editor built on imgui for editing the IP on the client if required: [Link](https://github.com/WerWolv/ImHex)

# Support
I have tested this on windows and on ubuntu server, if there are any fixes to be done, please raise a pull request.
I also want to learn how I can further harden the server via Docker, so any assistance is appreciated. 