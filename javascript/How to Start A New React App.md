  
# Setup Typescript with Node.js

Follow the instructions here: https://auth0.com/blog/node-js-and-typescript-tutorial-build-a-crud-api/

## 1. Create a project directory
```bash
mkdir <name_of_project>
cd <name_of_project>
```

## 2. Initialize Node.js project
This will create a `package.json` file, within the project directory with default settings:
`npm init -y`

## Installing Project Dependencies
`npm i express dotenv cors helmet`
- `express`: Fast unopinionated minimalist web framework for Node.js
- `dotenv`: Zero-dependency module that loads environment variables from a `.env` file int `process.env`
- `cors`: Express middleware to enable CORS with various options
- `helmet`: Express middleware to secure your apps by setting various HTTP headers, which mitigate common attack vectors.

## Installing Typescript to use Typescript

`npm i -D typescript`

## Installing Typescript dependencies
The following command installs the types available for the project dependencies we installed in the above steps. This installs the type definitions through the @type npm namepsace which hosts Typescript type definitions from the DefinitelyTyped project.

`npm i -D @types/node @types/express @types/dotenv @types/cors @types/helmet`

## Initializing Typescript in Node.js

We need a `tsconfig.json` file in order for the Typescript compiler to understand the project's structure.

`npm tsc --init`

## Install Postgres and Related Packages
`npm install sequelize pg pg-hstore`

`pg`: Postgres
`pg-hstore`: PostgresQL hstore format (https://www.ibm.com/cloud/blog/an-introduction-to-postgresqls-hstore)

## Install Postgres Typescript Dependencies

`npm install @types/pg @types/sequelize`
`npm install sequelize-typescript`


PG-hstore does not have a typescript dependency.

## 1. Setup your database

Using a database editor, create a database where the data will be located, a user that will access the database as well as the password to access it.

## 2. Create a config for PG

### 2a Populate your PG environment variables
Open up your `.env` file and enter something simiar:
```
PG_USERNAME = <postgres_username>
PG_PASSWORD = <postgres_password>
PG_DB = <name_of_database>
```


### 2b Create `db.config.ts`
To configure parameters for PostgresSQL connection and Sequelize, create a `db.config.ts` file, placed under a folder:  `api/src/config`
```shell
mkdir src/config
touch db.config.ts
```

### 2c Fill out the `db.config.ts`
```typescript
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

# REFERENCES

// https://auth0.com/blog/node-js-and-typescript-tutorial-build-a-crud-api/

// https://ultimatecourses.com/blog/setup-typescript-nodejs-express