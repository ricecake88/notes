Dockerize a React - Node - Postgres app

# Project Structure
> - **api**
> 	- **node_modules**
> 	- **src**
> 	- **js**
> 	- **apidocs**
> 	- **bin**
> 	- package-lock.json
> 	- package.json
> 	- README.md
> 	- Dockerfile.dev
> 	- Dockerfile
> 	- server.js
> 	- jest.config.js # for running tests
> 	- webpack.config.js # optional
> 	- tsconfig.json
> 	- tslint.json
> -**ui**
> 	-**node_modules**
> 	-**public**
> 	-**src**
> 	-**test**
> 	-**docs**
> 	-Dockerfile.dev
> 	-Dockerfile
> 	-.gitignore
> 	-package-lock.json
> 	-package.json
> 	-README.md
> 	-tsconfig.json
> 	-tslint.json
> 	-LICENSE.txt
> 	-postman_collection.json
> **postgresql**
> 	-.env
> 	-initdb.sql
> 	.gitignore
> .gitignore
> package-lock.json
> package.json
> docker-compose.yml
> docker-compose-dev.yml
> .prettierrc
> README.md
> 

References: 
[Docker-compose with React, Node, and PostgreSQL. A multi-container application with Docker](https://hardcoded.medium.com/docker-compose-with-react-node-and-postgresql-a-multi-container-application-with-docker-a11197802e33)
[Dockerize Node Postgresql](https://bestofreactjs.com/repo/alexeagleson-docker-node-postgres-template)

## Create folder that will be associated with your git repo.
## Make a directory for your front end app at the root
In Typescript:
	`npx create-react-app <name_of_app> --template typescript`
Resource:
https://create-react-app.dev/docs/getting-started/

## Create a Dockerfile for your frontend app
This file will be used to build the image. This file defines your configuration steps. More specifically it tells our container how to behave and install our dependencies and run the application

Create your Dockerfile for development:
`vim ui/Dockerfile.dev`

Create your Dockerfile for production:
`vim ui/Dockerfile`

Paste the following into Dockerfile. Remove any typescript references if not using typescript:
```dockerfile
FROM node:16.14.0

#By default create-react-app sets port to 3000 when you start react app, but I configured to run ui on 8080 (just personal preference) instead of 3000.

EXPOSE 8080   

RUN mkdir -p /app/public /app/src  
WORKDIR /app

COPY tsconfig.json /app/tsconfig.json #ignore if don't have react with typescript  
COPY tslint.json /app/tslint.json #ignore if don't have react with typescript

COPY package.json /app/package.json  
COPY package-lock.json /app/package-lock.json

## install only the packages defined in the package-lock.json (faster than the normal npm install)  
RUN npm install

# Run 'npm run dev' when the container starts.  
CMD ["npm", "run", "dev"]

```
### Start up front end app
```unix
cd <name_of_app>
npm start
```

## Make a directory for your backend app at the root
```ts
mkdir api
cd api
```

### Create a Dockerfile.dev
`touch Dockerfile.dev`

Paste the following into the file (Remove any typescript related files):
```dockerfile
FROM node:16.14.0  
RUN mkdir -p /app/config /app/src  
WORKDIR /app  
COPY tsconfig.json /app/tsconfig.json  
COPY tslint.json /app/tslint.json  
COPY package.json /app/package.json  
COPY package-lock.json /app/package-lock.json  
RUN npm install  
CMD ["npm", "run", "dev"]
```
## Make a directory for your database at the root
`mkdir postgresql`

## Create your docker-compose.yml
This file will allow us to define the configuration for each container more like passing commands such as `docker container create, docker volume create, docker network create`

`vim docker-compose.yml`

Enter the following here:
```yaml
version '1'
services:
	postgres:  
		image: postgres:12.1  
		ports:  
			- "5432:5432"  
		environment:  
			POSTGRES_PASSWORD: mypassword  
		volumes:  
			- ./postgresql/data:/var/lib/postgresql/data
	api:  
		build:  
			context: ./api  
			dockerfile: Dockerfile.dev  
		volumes:  
			- /app/node_modules  
			- ./service/config:/app/config  
			- ./service/src:/app/src  
			- ./service/test:/app/test  
		ports:  
			- "3000:3000"  
	ui:  
		build:  
			context: ./ui  
		dockerfile: Dockerfile.dev  
		volumes:  
			- /app/node_modules  
			- ./ui:/app  
		ports:  
			- "8080:8080"

```

## Setup Workspace

### Create API / Node JS Backend server

For reference:
https://blog.logrocket.com/the-perfect-architecture-flow-for-your-next-node-js-project/

Node & Typescript:
https://blog.appsignal.com/2022/01/19/how-to-set-up-a-nodejs-project-with-typescript.html

Node instructions:
https://blog.risingstack.com/node-hero-tutorial-getting-started-with-node-js/

To Run Node in git bash:
https://stackoverflow.com/questions/39322780/node-command-not-working-in-git-bash-terminal-on-windows

1. Create the folder that will be your backend / server app
```shell
mkdir <name_of_backend_folder>
cd <name_of_backend_folder>
npm init
```
 
2. Install Typescript, express, nodemon in our project by the following command. Am not sure if nodemon is necessary when watch changes each time there is a change.
````bash
npm i --save-dev typescript tslint nodemon @types/pg @types/express dotenv
````

3. Install express and postgresql
```shell
npm i pg express
```

3. Create the `package.json` and add:
```json
"script": {
    "start": "node ./dist/app.js",
    "dev": "nodemon -L -e ts --exec \"npm run build && npm start\"",
    "build": "tsc"
  }
```

`build` converts all the .ts files into .js and puts it into the dist folder if configured in the `tsconfig.json`
`dev` uses `nodemon` to watch for changes in any .ts file (-e ts). When there are changes, it will run the `build` & `start` scripts. 
'-L' is required when using `nodemon` in containers
`start` starts up our server.

4. At the root, create a `tsconfig.json` file. Paste the following (remove comments)
```json
{
	"extends": "@tssconfig/node16/tsconfig.json",
    "compilerOptions": {  
      "target": "es6" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
      "module": "commonjs" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
      "outDir": "./dist" /* Redirect output structure to the directory. */,
      "strict": true /* Enable all strict type-checking options. */,
      "typeRoots": ["./node_modules/@types"] /* List of folders to include type definitions from. */,
      "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
      "skipLibCheck": true /* Skip type checking of declaration files. */,
      "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */,
      "rootDir": "."
    },
    "include": ["src"],
    "exclude": ["node_modules"]
}
```
5. To test that Typescript is working, spin up tsc by entering in one command window:
`tsc`
6.  Install all the packages and dependencies from `package.json`:
```shell
npm install
```

If **Error: EACCES: permission denied** error occurs  

```shell
npm install -g --unsafe-perm=true --allow-root
```

8.  In the other window, start up npm by entering:
`npm start`

If you want to stop it, enter
`npm stop`


8. Create a .env file at the root so that we use the same variables when configuring Docker Compose and the server. Also we can hide the environment variables used in Docker Compose as `docker-compose.yml` are committed to Github, while the .env file is not.

```env
APP_NAME = node-project
NODE_ENV = development
PORT = 3003
JWT_SECRET = '$2a$0sdfwewewerbN2Jf2UcG'
JWT_KEY = 'ewrwetiLCJjb21wYW55IjoiQWlyN3NlYXMtwetPpcuKZCtFE4k'
LOCAL_DB_URI = mongodb://localhost:27017/db_api
SERVER_DB_URI = mongodb+srv://cezDbAdmin:cezDbAsdd@development-we.gcp.mongodb.net(opens in new tab)/cdez?retryWrites=true&w=majority

CONTACT_API_HOOK = http://api.node-app.dev/contactserver/api/v1
CONTACT_API_HOOK1 = http://127.0.0.1:3003/contactserver/api/v1

ALERT_MAIL = 'noreply@gmail.com(opens in new tab)'
FIREBASE_REDIRECT_URL='http://localhost:8080/dashboard'

MAIL_API_HOOK = http://api.node-project.dev/mailserver/api/v1
ALERT_MAIL='Node project <alerts@node-project.com>'
```


To deal with global being deprecated:
https://stackoverflow.com/questions/72401421/message-npm-warn-config-global-global-local-are-deprecated-use-loc
