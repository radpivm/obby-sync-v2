# basic version

To run just the couchdb container without any of the other stuff use the docker compose basic file - Note that version does not use variables all stuff must be declared in the docker-compose.yml file.
to use the basic version

```
mv docker-compose-basic.yml docker-compose.yml
docker compose up -d
```
go to confirming everything is working section below

# full version 

This version of the couchdb container uses env variables that must be set prior to launching in order for it to work

First we need to setup our caddy network


```
# start but running this on the host
docker network create caddy-network 



the variables that need to be set and what they are for is below

Set theses inside the .env file

environment variables 
- **_SERVER_URL_**: This is the complete URL that will be used to serve the CouchDB instance. Example: `[https://obsidian.example.com`](https://obsidian.example.com`/)
- **_COUCHDB_USER_**: The administrator user that will be used to manage your CouchDB instance. Example: `obsidian`
- **_COUCHDB_PASSWORD_**: The password for the CouchDB admin user.
- **_COUCHDB_DATABASE_**: The name of the database within CouchDB that will be used to contain your sync data for Obsidian.
- **COUCHDB_DATA**: An existing path on your host’s filesystem where the CouchDB data will be saved. This folder will have to be backed up regularly to safeguard your notes. 


once all of them are set up in your .env file  run docker compose up

### Confirming everythings working

once your container is up and running Confirm that your CouchDB application is running and has been correctly configured by navigating to the host IP on the CouchDB port. Log in and confirm that your settings have been applied.

Example: `[http://192.168.2.1:5941/_utils`]

Confirm that the following settings were applied (note that exact values may change over time):

- **enable_cors = true**
- **max_http_request_size = 4294967296**
- **max_document_size = 50000000**
- **require_valid_user = true**
- **origins = app://obsidian.md,capacitor://localhost,**[**http://localhost**](http://localhost%2A%2A/)
- **WWW-Authenticate = Basic realm=\”couchdb\”**


next you must create the usertable

The container expects the _user table to exist on your couchDB instance otherwise it will continuously throw errors you can create it with a call to CouchDB’s API via curl.

```# Replace "admin" and "password" with your CouchDB credentials created above  
curl -u admin:password -X PUT http://localhost:5984/_users
