#### There are 3 services in this application
1. Github Searcher Frontend Service ( [github-search-frontend](https://github.com/blocksan/github-search-frontend) )
2. Github Searcher Backend MicroService ( [github-search](https://github.com/blocksan/github-search) )
3. REDIS microservice ( [microservice-redis](https://github.com/blocksan/microservice-redis) )

Details about all the services are provided below with snapshots. Please read section by section.
***Refer : Running the project end to end via docker images [E2E]*** for running the project end to end or if you want to skip the documentation

# Github Searcher Frontend Service   
It is the frontend application which will serve the UI for the github searcher application built with ***infinite scroll*** and ***multi stage docker build***.
Following technologies have been used while building this application
#### Technologies used
1. ***Reactjs***
2. ***Redux***
3. ***Redux Thunk***
4. ***React Persist*** (session storage)
5. ***React Router***
6. ***SASS*** stylings
7. ***Font Awesome*** for icons
8. ***Jest Enzyme*** for unit testing
9. ***React Moment*** 
10. ***React Memoization*** 

#### Decisions made during the development
1. ***Infinite Scrolling*** - 
     - Github API results only 30 items at a time, hence to see the other results either we can use trivial solution i.e Pagination or Infinite Scroll. 
     - I Chose ***Infinite Scroll*** as the application is mobile friendly hence it will be easier for the user to swipe UP. 
2. ***Multi Stage Build*** 
     - I chose multi stage docker build for the frontend to make the container size very small and other advance advantages we can get like Zofli compression (Can be added in Extended Scope).
3. ***Session Storage*** -  
    -  To cache the API result we can either use local storage or session storage. I chose session storage as it will clear the store on browser close and tab close. 
    -  By clearing the frontend store, we can easily simulate and see the performance enhancement from the REDIS cache which is implemented at backend. 

#### Screen sizes available
1. UHD Desktop - 2560*
2. Large Screen - 1440*
3. Medium Screen - 1280*
4. Ipad/Small Screen - 768*
5. Mobile Screen - 380* | 425*


### Following are the some snapshots of the application
##### Home page
[![dPTiWN.md.png](https://iili.io/dPTiWN.md.png)](https://freeimage.host/i/dPTiWN)

##### Loading screen
[![dPTssI.md.png](https://iili.io/dPTssI.md.png)](https://freeimage.host/i/dPTssI)

##### User results
[![dPTQft.md.png](https://iili.io/dPTQft.md.png)](https://freeimage.host/i/dPTQft)

##### User results hover
[![dPTZ0X.md.png](https://iili.io/dPTZ0X.md.png)](https://freeimage.host/i/dPTZ0X)

##### Repository results
[![dPTtgn.md.png](https://iili.io/dPTtgn.md.png)](https://freeimage.host/i/dPTtgn)

##### Repository results hover
[![dPTm5G.md.png](https://iili.io/dPTm5G.md.png)](https://freeimage.host/i/dPTm5G)

##### Infinite Scrolling
[![dPTpef.md.png](https://iili.io/dPTpef.md.png)](https://freeimage.host/i/dPTpef)

##### Screen size 425
[![dPT4bR.md.png](https://iili.io/dPT4bR.md.png)](https://freeimage.host/i/dPT4bR)

##### Screen size 768
[![dPTPxp.md.png](https://iili.io/dPTPxp.md.png)](https://freeimage.host/i/dPTPxp)

##### Test Cases
[![diBmLF.md.png](https://iili.io/diBmLF.md.png)](https://freeimage.host/i/diBmLF)

#### Features included in this backend service
  -  **Infinite Scrolling** 
  -  **Store Rehydration** using React Persist
  -  **API Caching** using sessionStorage
  -  **JSDoc Comments** for the internal components
  -  **Component Based structure** 
  -  **Test Cases** (action and reduces)
  -  **Dockerized** container with development mode
  -  **Dockerized** container with production mode

#### Available Components
  -  HeaderComponent
  -  ContentComponent
  -  ComponentsWrapper
  -  RepositoryCardComponent
  -  UserCardComponent
  -  SearchComponent
  -  SearchWrapperComponent
  -  HeaderWrapperComponent

### How to run this project
### Option 1 - Without Docker
##### Run project [ without ] ~~DOCKER~~
**Node/NPM should be preinstalled in the host machine
Run the following commands
```
git clone https://github.com/blocksan/github-search-frontend.git

cd github-search-frontend

touch .env
    -  Add the base url for the server in the .env file
yarn install

npm install -g env-cmd

yarn start
    Open [http://localhost:3000](http://localhost:3000) to view it in the browser.
yarn test
    Launches the test runner in the interactive watch mode.
````

### Option 3 - With Docker
This project contains 2 versions of Dockerfile 
1. ***Dockerfile***  Production
2. ***Dockerfile.dev*** Development (with hot reloading)
##### Run project [ with ] DOCKER
Run the following commands

```
git clone https://github.com/blocksan/github-search-frontend.git

cd github-search-frontend

touch .env
    -  Add the base url for the server in the .env file
docker build -t github-search-frontend:latest .

docker run -p 3000:3000 github-search-frontend
```
Open [http://localhost:3000](http://localhost:3000) to view the running application in the browser

# Github Searcher Backend MicroService   
It is responsible to provide the endpoints which can be used by the consumer.
This service is built with NestJs framework ( express ) with Event Pattern Microservice Architecture.
Features included in this backend service
  -  **Swagger documentation** for APIs - path : ip_address/api/gitswagger
  -  **Compodoc documentation** for the whole application - path : ip_address:8000
  -  **Custom Logger service**
  -  **Custom HTTP Exception service**
  -  **Event/Message based pub-sub** channel with **REDIS microservice**
  -  **Cache and Content** details API
  -  **Request Middleware** - adds unique request Id to each request
  -  **Dockerized** container with development mode
  -  **Dockerized** container with production mode
  -  **TsDoc** based documentation for the controllers and services
  -  **Test Cases** for the API endpoint

#### Decisions made during the development
1. ***Nestjs Framework*** 
     - Though the application could have been built with trivial express server but if we think about maintaibility, scalibility, documentation, microservices all these are important factors in a bigger projects. 
     - Hence ***Nestjs*** provides one of the best solution for all the above points. [ Similar work could have been done with Typestack which includes Rounting Controllers, TypeORM, Class Validators, etc... ]
2. ***Microservice Architecture*** 
     - Instead of building a monolith application which can have single point of failure, vertical scaling would be nearly impossible at one point of time and large codebase kind of issues in future.
     - Hence I decided to go with Microservice architecture in which 
       -  ***APP Service Container (AMC)*** only responsible for serving the API
       -  ***REDIS Service Container (RMS)*** only responsible for maintaing the cache
     - With microservice approach we can scale our individual services horizontally, with high fault tolerant and higher availability with Load Balancer, higher throughput. (All can be a extended scope with current architecture)

#### High level design of the application
[![dPAxvs.md.png](https://iili.io/dPAxvs.md.png)](https://freeimage.host/i/dPAxvs)

#### components in the application
1. ***APP Service Container*** (AMC)
   - Responsible for serving all the APIs
   - Sends Event to REDIS Microservice (Fire and Forget Strategy) to cache the content.
   - Sends Message Pattern to REDIS Microservice to fetch the cache content.
   - ***Swagger*** documentation is available with the project
2. ***APP Documentation Container***
   - TSDoc commenting style has been used across the application
   - ***Compodoc*** documentation manager is integrate to generate the documentation. 
3. ***Redis Container***
   - Runs a container containing REDIS on Port 6379
   - APP Microservie Container and REDIS Microservice Container connects with this container
4. ***Redis Microservice Container*** (RMC)
   - Microservice just to communicate with REDIS and perform actions on REDIS container
   - It can handle both Event Pattern and Message Pattern
5. ***Redis Microservice Compodoc Container***
   - TSDoc commenting style has been used across the application
   - ***Compodoc*** documentation manager is integrate to generate the documentation. 
6. ***Frontend Container***
   - It is a multistage build container which first builds the raw application and then only static folder is served by intermediate NGINX container
7. ***Nginx Container***
   - Works as reverse proxy and sits in front of all the containers.
   - Responsible for serving static content as well as for routing API endpoints.

#### Swagger Documentation snapshot
[![dPYKRn.md.png](https://iili.io/dPYKRn.md.png)](https://freeimage.host/i/dPYKRn)

#### Compodoc Documentation snapshot
[![dPYCxf.md.png](https://iili.io/dPYCxf.md.png)](https://freeimage.host/i/dPYCxf)

#### Test Cases
[![diBbX1.md.png](https://iili.io/diBbX1.md.png)](https://freeimage.host/i/diBbX1)

## How to run this application
  - Option 1 : Github way of cloning repositories 
  - Option 2 : Directly building the application from docker-hub
  - Option 3 : Building the docker images from local code 
## Installation

#### Option 1:  Running project locally
* NPM/Node should be preinstalled in the host machine
* Redis should be installed and running in the host machine
* Please add **github auth token** to make more than the limited github calls 
Follow the below commands to run project locally
```
git clone https://github.com/blocksan/github-search.git

cd github-search

touch .env
    - Add the values for APP_PORT, ROUTE_PREFIX, REDIS_SERVER, GIT_AUTH_TOKEN (optional), SWAGGER_PATH
yarn install

yarn start:dev

yarn run doc 
    -  visit (http://localhost:8080) to view the documentation
    -  visit (http://localhost/api/gitswagger) for swagger documentation
    
cd ..
    - To run REDIS microservice, change to previous directory and follow below commands
git clone https://github.com/blocksan/microservice-redis.git

cd microservice-redis

touch .env
    - Add the value for REDIS_SERVER, REDIS_CLIENT_NAME, REDIS_EXPIRY
    
yarn install

yarn start:dev
```

Test
```bash
# unit tests
$ yarn run test
```

#### Option 2:  Running project via docker images
To run the project using docker please use the ***docker-compose.deploy.yml*** provided with the project
Follow the below steps to run the application
1. Create ***frontend.env*** for frontend container and add values
2. Create ***app.env*** for APP container and add relevant values
3. Create ***microservice.env*** for REDIS Microservice container and add relevant values.
```
docker-compose -f docker-compose.deploy.yml build
docker-compose -f docker-compose.deploy.yml up
```
Open [http://host_ip](http://localhost) to view it in the browser.
Open [http://host_ip:8000](http://localhost:8000) to view APP documentationin the browser
Open [http://host_ip:9000](http://localhost:9000) to view REDIS Microservice documentation in the browser
Open [http://host_ip/api/swagger](http://localhost/api/gitswagger) to view swagger documentation in the browser

#### Option 3:  Running project via docker images
```
git clone https://github.com/blocksan/github-search.git

cd github-search

touch .env
    - Add the values for APP_PORT, ROUTE_PREFIX, REDIS_SERVER, GIT_AUTH_TOKEN (optional), SWAGGER_PATH
    
cd ..
    - To run REDIS microservice, change to previous directory and follow below commands
    
git clone https://github.com/blocksan/microservice-redis.git

cd microservice-redis

touch .env
    - Add the value for REDIS_SERVER, REDIS_CLIENT_NAME, REDIS_EXPIRY
```
#### Production environment
- docker-compose -f docker-compose.yml build
    - This will generate the production docker for the project
- docker-compose -f docker-compose.yml up
    - This will up the application in production environment

#### Development environment (Hot Reloading)
- docker-compose -f docker-compose.dev.yml build
    - This will generate the development docker for the project
- docker-compose -f docker-compose.dev.yml up
    - This will up the application in development environment

# REDIS MicroService   
It is responsible to caching and serving the content.
This service is built with NestJs framework with Event Pattern Microservice Architecture.
Features included in this backend service
  -  **Compodoc documentation** for the whole application - path : ip_address:8000
  -  **Custom Logger service**
  -  **Custom HTTP Exception service**
  -  **Event/Message Pattern**
  -  **Dynamic Caching** Auto clear the cache in 2hr (default) configurable value 
  -  **Request Middleware** - adds unique request Id to each request
  -  **Dockerized** container with development mode
  -  **Dockerized** container with production mode
  -  **TsDoc** based documentation for the controllers and service

#### Decisions made during the development
1. ***Nestjs Framework*** 
     - Though the application could have been built with trivial express server but if we think about maintaibility, scalibility, documentation, microservices all these are important factors in a bigger projects. 
     - Hence ***Nestjs*** provides one of the best solution for all the above points. [ Similar work could have been done with Typestack which includes Rounting Controllers, TypeORM, Class Validators, etc... ]
2. ***Microservice Architecture*** 
     - Instead of building a monolith application which can have single point of failure, vertical scaling would be nearly impossible at one point of time and large codebase kind of issues in future.
     - Hence I decided to go with Microservice architecture in which 
       -  ***APP Service Container (AMC)*** only responsible for serving the API
       -  ***REDIS Service Container (RMS)*** only responsible for maintaing the cache
     - With microservice approach we can scale our individual services horizontally, with high fault tolerant and higher availability with Load Balancer, higher throughput. (All can be a extended scope with current architecture)

#### High level design of the application
[![dPAxvs.md.png](https://iili.io/dPAxvs.md.png)](https://freeimage.host/i/dPAxvs)

#### components in the application
1. ***APP Service Container*** (AMC)
   - Responsible for serving all the APIs
   - Sends Event to REDIS Microservice (Fire and Forget Strategy) to cache the content.
   - Sends Message Pattern to REDIS Microservice to fetch the cache content.
   - ***Swagger*** documentation is available with the project
2. ***APP Documentation Container***
   - TSDoc commenting style has been used across the application
   - ***Compodoc*** documentation manager is integrate to generate the documentation. 
3. ***Redis Container***
   - Runs a container containing REDIS on Port 6379
   - APP Microservie Container and REDIS Microservice Container connects with this container
4. ***Redis Microservice Container*** (RMC)
   - Microservice just to communicate with REDIS and perform actions on REDIS container
   - It can handle both Event Pattern and Message Pattern
5. ***Redis Microservice Compodoc Container***
   - TSDoc commenting style has been used across the application
   - ***Compodoc*** documentation manager is integrate to generate the documentation. 
6. ***Frontend Container***
   - It is a multistage build container which first builds the raw application and then only static folder is served by intermediate NGINX container
7. ***Nginx Container***
   - Works as reverse proxy and sits in front of all the containers.
   - Responsible for serving static content as well as for routing API endpoints.

#### Compodoc Documentation snapshot
[![dPYCxf.md.png](https://iili.io/dPYCxf.md.png)](https://freeimage.host/i/dPYCxf)

## How to run this application
  - Option 1 : Github way of cloning repositories 
  - Option 2 : Directly building the application from docker-hub
  - Option 3 : Building the docker images from local code 
## Installation

#### Option 1:  Running project locally
* NPM/Node should be preinstalled in the host machine
* Redis should be installed and running in the host machine
* Please add **github auth token** to make more than the limited github calls 
Follow the below commands to run project locally
```
git clone https://github.com/blocksan/github-search.git

cd github-search

touch .env
    - Add the values for APP_PORT, ROUTE_PREFIX, REDIS_SERVER, GIT_AUTH_TOKEN (optional), SWAGGER_PATH
yarn install

yarn start:dev

yarn run doc 
    -  visit (http://localhost:8080) to view the documentation
    -  visit (http://localhost/api/gitswagger) for swagger documentation
    
cd ..
    - To run REDIS microservice, change to previous directory and follow below commands
git clone https://github.com/blocksan/microservice-redis.git

cd microservice-redis

touch .env
    - Add the value for REDIS_SERVER, REDIS_CLIENT_NAME, REDIS_EXPIRY
    
yarn install

yarn start:dev
```

#### Option 2:  Running project via docker images
To run the project using docker please use the ***docker-compose.deploy.yml*** provided with the project
Follow the below steps to run the application
1. Create ***frontend.env*** for frontend container and add values
2. Create ***app.env*** for APP container and add relevant values
3. Create ***microservice.env*** for REDIS Microservice container and add relevant values.
```
docker-compose -f docker-compose.deploy.yml build
docker-compose -f docker-compose.deploy.yml up
```
Open [http://host_ip](http://localhost) to view it in the browser.
Open [http://host_ip:8000](http://localhost:8000) to view APP documentationin the browser
Open [http://host_ip:9000](http://localhost:9000) to view REDIS Microservice documentation in the browser
Open [http://host_ip/api/swagger](http://localhost/api/gitswagger) to view swagger documentation in the browser

#### Option 3:  Running project via docker images
```
git clone https://github.com/blocksan/github-search.git

cd github-search

touch .env
    - Add the values for APP_PORT, ROUTE_PREFIX, REDIS_SERVER, GIT_AUTH_TOKEN (optional), SWAGGER_PATH
    
cd ..
    - To run REDIS microservice, change to previous directory and follow below commands
    
git clone https://github.com/blocksan/microservice-redis.git

cd microservice-redis

touch .env
    - Add the value for REDIS_SERVER, REDIS_CLIENT_NAME, REDIS_EXPIRY
```
#### Production environment
- docker-compose -f docker-compose.yml build
    - This will generate the production docker for the project
- docker-compose -f docker-compose.yml up
    - This will up the application in production environment

#### Development environment (Hot Reloading)
- docker-compose -f docker-compose.dev.yml build
    - This will generate the development docker for the project
- docker-compose -f docker-compose.dev.yml up
    - This will up the application in development environment


## Running the project end to end via docker images
```
git clone https://github.com/blocksan/github-searcher-docker-compose.git

cd github-searcher-docker-compose
    - change the values for .env files as per your need

docker-compose -f docker-compose.deploy.yml build

docker-compose -f docker-compose.deploy.yml up
```
Open [http://host_ip](http://localhost) to view the app in the browser.

Open [http://host_ip:8000](http://localhost:8000) to view APP documentation the browser.

Open [http://host_ip:9000](http://localhost:9000) to view REDIS Microservice documentation in the browser.

Open [http://host_ip/api/gitswagger](http://localhost/api/gitswagger) to view swagger documentation in the browser.


#### Stay in touch
 [Sandeep Ghosh](http://sandeepghosh.com)

