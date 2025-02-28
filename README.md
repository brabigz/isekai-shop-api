Isekai Shop API

Introduction


About Isekai Shop API
This course project is named “Isekai Shop API.” “Isekai” means another world (If you are an anime fan, you probably already know this world, lol), So this project is going to act like CRUD project + OAuth2.

Features of Isekai Shop API
Item Shop
Listing
Selling
Buying
Item Managing
Creating
Editing
Archiving
Inventory
Listing
Player Coin
Coin Adding
Showing
OAuth2
Player Login
Admin Login
Logout
Middleware
Player Authorize
Admin Authorize
Architecture
alt text

ER Diagram
alt text

Start PostgreSQL on Docker
Pull the PostgreSQL image

docker pull postgres:alpine
Start the PostgreSQL container

docker run --name isekaishopdb -p 5432:5432 -e POSTGRES_PASSWORD=123456 -d postgres:alpine
Create the Isekai Shop Database

docker exec -it isekaishopdb bash
psql -U postgres
CREATE DATABASE isekaishopdb;
In case you need to delete the database

DROP DATABASE isekaishopdb;
Database Migration
go run ./databases/migration/migratedb.go
config.yaml Example
server:
  port: 8080
  allowOrigins:
    - "*"
  bodyLimit: "10M" # MiB
  timeout: 30 # Seconds

oauth2:
  playerRedirectUrl: "http://localhost:8080/v1/oauth2/google/player/login/callback"
  adminRedirectUrl: "http://localhost:8080/v1/oauth2/google/admin/login/callback"
  clientId: "xxxxx.apps.googleusercontent.com"
  clientSecret: "xxxxx"
  endpoints:
    authUrl: "https://accounts.google.com/o/oauth2/auth?access_type=offline&approval_prompt=force"
    tokenUrl: "https://oauth2.googleapis.com/token"
    deviceAuthUrl: "https://oauth2.googleapis.com/device/code"
  scopes:
    - "https://www.googleapis.com/auth/userinfo.email"
    - "https://www.googleapis.com/auth/userinfo.profile"
  userInfoUrl: "https://www.googleapis.com/oauth2/v2/userinfo"
  revokeUrl: "https://accounts.google.com/o/oauth2/revoke"
  
database:
  host: localhost
  port: 5432
  user: postgres
  password: 123456
  dbname: isekaishopdb
  sslmode: disable
  schema: public
Start Isekai Shop API using Docker
Let's see the IPv4 of our database container first by this follwing command.

docker network inspect bridge
Then copy the IPv4 of isekaishopdb to change the host of database in the config.yaml.

And now let's build and start the isekai-shop-api through the Docker.

docker build -t isekai-shop-api:v1.0.0 .
docker run --name isekai-shop-api -v /path/to/config-folder:/app/etc -d isekai-shop-api:v1.0.0
Postman Collection and ENV
Collection
