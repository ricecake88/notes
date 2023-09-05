  
# A. Setup Typescript with Node.js - The API

Follow the instructions here: https://auth0.com/blog/node-js-and-typescript-tutorial-build-a-crud-api/

## 1. Create a project directory
```bash
mkdir <name_of_project>
cd <name_of_project>
```

## 2. Installing Packages Necessary

### 2.1 Initialize Node.js project
This will create a `package.json` file, within the project directory with default settings:
`npm init -y`

### 2.2 Installing Project Dependencies
`npm i express dotenv cors helmet`
- `express`: Fast unopinionated minimalist web framework for Node.js
- `dotenv`: Zero-dependency module that loads environment variables from a `.env` file int `process.env`
- `cors`: Express middleware to enable CORS with various options
- `helmet`: Express middleware to secure your apps by setting various HTTP headers, which mitigate common attack vectors.

### 2.3 Installing Typescript to use Typescript

`npm i -D typescript`

### 2.4 Installing Typescript dependencies
The following command installs the types available for the project dependencies we installed in the above steps. This installs the type definitions through the @type npm namepsace which hosts Typescript type definitions from the DefinitelyTyped project.

`npm i -D @types/node @types/express @types/dotenv @types/cors @types/helmet`

### 2.5 Install Postgres and Related Packages
`npm install sequelize pg pg-hstore sequelize-cli -g`

`pg`: Postgres
`pg-hstore`: PostgresQL hstore format (https://www.ibm.com/cloud/blog/an-introduction-to-postgresqls-hstore)

### 2.6 Install Postgres Typescript Dependencies (and Sequelize library)

`npm install @types/pg
`npm install sequelize-typescript`

PG-hstore does not have a typescript dependency.

## 3. Initializing Typescript in Node.js

We need a `tsconfig.json` file in order for the Typescript compiler to understand the project's structure.

`npm tsc --init`

## 4. Use Environment Variables

Populate `.env` with the following variables that defines the port your server listens on for requests:
```typescript
PORT=7000
```

Make sure to add `.env` file to your `.gitignore`

## 5. Setting up your database

Using your database editor, create a database where the data will be located, a user that will access the database as well as the password to access it.

### 5.1 Create a config for PG

#### 5.1.1 Populate your PG environment variables
Open up your `.env` file and enter something simiar:
```typescript
PG_USERNAME = <postgres_username>
PG_PASSWORD = <postgres_password>
PG_DB = <name_of_database>
```

#### 5.1.2 Create `db.config.ts`
To configure parameters for PostgresSQL connection and Sequelize, create a `db.config.ts` file, placed under a folder:  `api/src/config`
```shell
mkdir src/config
touch db.config.ts
```

#### 5.1.3 Fill out the `db.config.ts`
```typescript
import * as dotenv from 'dotenv';

// load the environment variables
dotenv.config();

export const dbConfig = {
  HOST: "localhost",
  USER: process.env.PG_USERNAME,
  PASSWORD: process.env.PG_PASSWORD,
  DB: process.env.PG_DB,
  dialect: "postgres",
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  }
};
```

First five parameters are for PostgreSQL connection.  
`pool` is optional, it will be used for Sequelize connection pool configuration:
-   `max`: maximum number of connection in pool
-   `min`: minimum number of connection in pool
-   `idle`: maximum time, in milliseconds, that a connection can be idle before being released
-   `acquire`: maximum time, in milliseconds, that pool will try to get connection before throwing error

## 6. Connect to a database

### 6.1 Sequelize

### 6.1.1 Import and create connection to database using Sequelize

Under `src/models/index.ts`, we want to pass in our database connection information that we we filled out in the `src/config/db.config.ts` file.

```typescript
import { Dialect, Sequelize } from 'sequelize';
import { dbConfig } from '../config/db.config';

const sequelizeConnection = new Sequelize(
    dbConfig.DB,
    dbConfig.USER,
    dbConfig.PASSWORD,
    {    
        dialect: dbConfig.dialect as Dialect,
        port: dbConfig.PORT,
        host: dbConfig.HOST
    }
)
export default sequelizeConnection;
```

#### 6.1.2 Create Migrations.
Since Sequelize doesn't have a great way to create these migrations using typescript since they are all using `sequelize cli`, we need to do the following:

Create a `.sequelizerc` to a `dist/sequelize`

```typescript
const { resolve } = require('path');

module.exports = {
    config: resolve('dist/config.js'),
    'seeders-path': resolve('dist/seeders'),
    'migrations-path': resolve('dist/migrations'),
    'models-path': resolve('dist/models')
}
```

#### 6.1.3 Create  a table

The following command will place this in under the `api/migrations` folder, but it will be a \*.js file.
`sequelize migration:create --name name-of-table-to-add`

#### 6.1.4 Replace the contents of generated file

The file generated is only in Javascript, so it needs to be typed:
```typescript
import { QueryInterface, DataTypes, QueryTypes } from 'sequelize';

module.exports = {
    up: (queryInterface: QueryInterface): Promise<void> => queryInterface.sequelize.transaction(
        async (transaction) => {
          // here go all migration changes
        }
    ),

    down: (queryInterface: QueryInterface): Promise<void> => queryInterface.sequelize.transaction(
        async (transaction) => {
          // here go all migration undo changes
        }
    )
};
```



## 3b. Connect to database directly to Postgres

Follow: https://northflank.com/guides/connecting-to-a-postgresql-database-using-node-js
and:
https://www.makeuseof.com/node-postgresql-connect-how/

# REFERENCES

Building a Crud App with Error Handling and auth check:
// https://auth0.com/blog/node-js-and-typescript-tutorial-build-a-crud-api/

// https://ultimatecourses.com/blog/setup-typescript-nodejs-express

Dockerizing Postgres
// https://dev.to/chema/how-to-create-a-dockerized-postgres-db-3hg9
// https://www.docker.com/blog/how-to-use-the-postgres-docker-official-image/
https://github.com/mghextreme/typescript-api-template/blob/master/docker-compose.yml
https://dev.to/antoniovdlc/a-look-at-postgresql-migrations-in-node-4gjp

Sequelize Links
// 
https://medium.com/@mghextreme/api-setup-using-nodejs-typescript-postgres-and-sequelize-48a0af72dda6
https://sequelize.org/docs/v6/other-topics/typescript/
https://dev.to/chandrapantachhetri/docker-postgres-node-typescript-setup-47db
https://archive.ph/F83qA
https://sequelize.org/docs/v6/core-concepts/model-basics/
https://sequelize.org/docs/v6/getting-started/
https://codeforgeek.com/getting-started-sequelize-postgresql/
https://sequelize.org/docs/v6/other-topics/migrations/
https://blog.logrocket.com/using-sequelize-with-typescript/
https://sequelize.org/docs/v6/
https://sequelize.org/api/v6/identifiers.html