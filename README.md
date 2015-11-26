Sunshot - Bright Harvest Application
=========

## Overview ##
This application is divided into 2 parts: **Front-end web application** and **Restful API Server**. Server-side application is a NodeJS server built on top of [Express.js](http://expressjs.com/) framework. Client-side application is a HTML/CSS/JS Single-Page Application (SPA) built on top of [AngularJS](https://angularjs.org/) framework.

## Getting Setup ##
To setup the server, you'll need [node](http://nodejs.org), the recommended version is 1.12.x, but latest version should be fine. You will also need a globally installed [grunt-cli](http://gruntjs.com/getting-started) and [http-server](https://www.npmjs.com/package/http-server).

Please execute following under root account or use `sudo`
```
npm install -g grunt-cli http-server
```

Then browse the application folder and install all dependencies at `node` directory
```
cd node
npm install
```

## Database Setup ##
The server uses on MySQL database, so if you don't have one, please download and [install](http://www.mysql.com) MySQL database. 

For Ubuntu, you can run `apt-get`:
```
sudo apt-get install mysql-server
```
After installation, you can use MySQL [workbench](https://dev.mysql.com/downloads/workbench/) to connect and create database schemas.

## Environment Variable ##
Plase make sure to set and source environment variables before migrating database or starting the server. An example of `.env` file can be found at `node\env_sample`. For more detail on configurable environment variables, please see **Server Configuration** below.

Run the following command to source your `.env`

```
source your_config.env
```

## Database Migration ##
To migrate database tables, you will need to run the following command:

```
grunt dbmigrate
```
In case you want to undo the migration, please run:
```
grunt dbdown
```

Be warn that these `dbmigrate` execute every migrations inside folder `node\datasource\schema-migrations`, including test data. 

For production migration, please make sure database is clean by executing `dbdown` before removing the `20151126173400-test-data.js` file from the folder.

## Server Configuration ##
Default server configurations resides in `node\config\default.json`. The followings are configurable variables: 

|                            |                                                                                                                                                                                                                                                    |                                                  | 
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------| 
| Name                       | Description                                                                                                                                                                                                                                        | Value                                            | 
| WEB_SERVER_PORT            | Application web server port. The http server will listen on this port for connections.                                                                                                                                                             | 4000                                             | 
| SALT_WORK_FACTOR           | Work factor while generating salt for password hashing. This should be an integer value                                                                                                                                                            | 10                                               | 
| MODELS_DIRECTORY           | The directory where all the sequelize models are stored. This configuration is needed while instantiating datasource. This value is relative to the path at which node process is running. Generally this is the root of the source code directory | ./models                                         | 
| TOKEN_EXPIRATION_IN_MILLIS | Milliseconds passed since the token is expired. This value will be added to the current time value when token is generated.                                                                                                                        | 5184000000                                       | 
| DEFAULT_TOKEN_LENGTH       | Length of the reset password token in bytes. Random data of this much bytes is generated and used as reset password token                                                                                                                          | 48                                               | 
| DEFAULT_TOKEN_EXPIRY       | Milliseconds passed since the reset password token will be expired. This value will be added to the current time when token is generated                                                                                                           | 86400000                                         | 
| DEFAULT_SORT_COLUMN        | Default sort column to use for list API’s                                                                                                                                                                                                          | updatedAt                                        | 
| DEFAULT_LIMIT              | Default limit to use for list API’s                                                                                                                                                                                                                | 10                                               | 
| DEFAULT_SORT_DIRECTION     | Default sort direction for list API’s                                                                                                                                                                                                              | DESC                                             | 
| FILE_UPLOAD_DIRECTORY      | Temporary directory to upload files. This value is relative to the path at which node process is running. Generally this is the root of the source code directory                                                                                  | ./uploads                                        | 
| GOOGLE_PROJECT_ID          | id of the google cloud project. The application uses google cloud storage to store the files. This is the id of the google cloud project.                                                                                                          | sunshot-bright-harvest                           | 
| FORGOT_PASSWORD_LINK       | This link will be send to the user’s email when they forgot password. The :token will be replaced with the reset password token                                                                                                                    | http://localhost:4000/resetpassword?token=:token | 
| GOOGLE_REDIRECT_URL        | This is the REDIRECT_URL for google oauth2 authentication                                                                                                                                                                                          | http://localhost:4000/login/google/callback      | 
| FROM_EMAIL                 | The from email to use when sending out the emails                                                                                                                                                                                                  | admin@sunshot.com                                | 


Other than above configurations, some sensitive configurations must be set explicitly as environment variables. 

The followings are the configurable environment variables:

|                      |                                              | 
|----------------------|----------------------------------------------| 
| Name                 | Description                                  | 
| JWT_SECRET           | Jwt secret to use while generating jwt token | 
| MYSQL_CONNECTION_URI |                                              | 
|                      | mysql connection string                      | 
| SMTP_HOST            | Smtp host name to send the emails            | 
| SMTP_PORT            | Smtp port number to send the emails          | 
| SMTP_USERNAME        | Smtp username to use to send the emails      | 
| SMTP_PASSWORD        | Smtp password to use when sending the emails | 
| GOOGLE_CLIENT_ID     | Google oauth2 client id                      | 
| GOOGLE_CLIENT_SECRET | Google oauth2 client secrect                 | 

## Web Configuration ##
Default web configuration resides in `.\angularjs\js\constants.js`
|        |                                                                                                                                                                                                        |                       | 
|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------| 
| Name   | Description                                                                                                                                                                                            | Default               | 
| apiUrl | Root url of the corresponding API Server. This url should not contains the backslash at the end of url. (I.e, http://localhost:4000/ is not an appropriate root url, please use http://localhost:4000) | http://localhost:4000 | 

## Run the Server ##
To start the server, please execute the following command at `.\node`
```
npm start
```

## Run the Web ##
Make sure to point the web application to correct API Server by setting `apiUrl` at `angularjs\js\constants.js`.

To start web app, please execute the following command at `angularjs`
```
cd angularjs
http-server -p 8000
```
The web app will be available at `http://localhost:8000`.

## Copyright ##
Copyright (c) 2015, Topcoder Inc. All rights reserved.
