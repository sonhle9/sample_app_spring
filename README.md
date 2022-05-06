# RESTful API Spring Server Boilerplate

[![Build Status](https://travis-ci.org/hagopj13/node-express-boilerplate.svg?branch=master)](https://travis-ci.org/hagopj13/node-express-boilerplate)
[![Coverage Status](https://coveralls.io/repos/github/hagopj13/node-express-boilerplate/badge.svg?branch=master)](https://coveralls.io/github/hagopj13/node-express-boilerplate?branch=master)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

A boilerplate/starter project for quickly building RESTful APIs using Spring and MySQL.

By running a single command, you will get a production-ready Node.js app installed and fully configured on your machine. The app comes with many built-in features, such as authentication using JWT, request validation, unit and integration tests, continuous integration, docker support, API documentation, pagination, etc. For more details, check the features list below.

## Quick Start

To create a project, simply run:

```bash
git clone https://github.com/spring-guides/gs-spring-boot.git <project-name>
```

Or

> [https://start.spring.io/](https://start.spring.io/)

## Manual Installation

If you would still prefer to do the installation manually, follow these steps:

Clone the repo:

```bash
git clone --depth 1 https://github.com/bezkoder/spring-boot-refresh-token-jwt.git
cd spring-boot-refresh-token-jwt
```

Install the dependencies:

```bash
mvn spring-boot:run
```

Set the environment variables:

```bash
cp src/main/resources/.application.properties.example application.properties

# open .env and modify the environment variables (if needed)
```

## Table of Contents

- [Features](#features)
- [Commands](#commands)
- [Environment Variables](#environment-variables)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Error Handling](#error-handling)
- [Validation](#validation)
- [Authentication](#authentication)
- [Authorization](#authorization)
- [Logging](#logging)
- [Custom Mongoose Plugins](#custom-mongoose-plugins)
- [Linting](#linting)
- [Contributing](#contributing)

## Features

- **NoSQL database**: [MongoDB](https://www.mongodb.com) object data modeling using [Mongoose](https://mongoosejs.com)
- **Authentication and authorization**: using [passport](http://www.passportjs.org)
- **Validation**: request data validation using [Joi](https://github.com/hapijs/joi)
- **Logging**: using [winston](https://github.com/winstonjs/winston) and [morgan](https://github.com/expressjs/morgan)
- **Testing**: unit and integration tests using [Jest](https://jestjs.io)
- **Error handling**: centralized error handling mechanism
- **API documentation**: with [swagger-jsdoc](https://github.com/Surnet/swagger-jsdoc) and [swagger-ui-express](https://github.com/scottie1984/swagger-ui-express)
- **Process management**: advanced production process management using [PM2](https://pm2.keymetrics.io)
- **Dependency management**: with [Yarn](https://yarnpkg.com)
- **Environment variables**: using [dotenv](https://github.com/motdotla/dotenv) and [cross-env](https://github.com/kentcdodds/cross-env#readme)
- **Security**: set security HTTP headers using [helmet](https://helmetjs.github.io)
- **Santizing**: sanitize request data against xss and query injection
- **CORS**: Cross-Origin Resource-Sharing enabled using [cors](https://github.com/expressjs/cors)
- **Compression**: gzip compression with [compression](https://github.com/expressjs/compression)
- **CI**: continuous integration with [Travis CI](https://travis-ci.org)
- **Docker support**
- **Code coverage**: using [coveralls](https://coveralls.io)
- **Code quality**: with [Codacy](https://www.codacy.com)
- **Git hooks**: with [husky](https://github.com/typicode/husky) and [lint-staged](https://github.com/okonet/lint-staged)
- **Linting**: with [ESLint](https://eslint.org) and [Prettier](https://prettier.io)
- **Editor config**: consistent editor configuration using [EditorConfig](https://editorconfig.org)

## Commands

Running locally:

```bash
yarn dev
```

Running in production:

```bash
yarn start
```

Testing:

```bash
# run all tests
yarn test

# run all tests in watch mode
yarn test:watch

# run test coverage
yarn coverage
```

Docker:

```bash
# run docker container in development mode
yarn docker:dev

# run docker container in production mode
yarn docker:prod

# run all tests in a docker container
yarn docker:test
```

Linting:

```bash
# run ESLint
yarn lint

# fix ESLint errors
yarn lint:fix

# run prettier
yarn prettier

# fix prettier errors
yarn prettier:fix
```

## Environment Variables

The environment variables can be found and modified in the `.env` file. They come with these default values:

```bash
# Port number
PORT=8080

# URL of the MySQL DB
spring.datasource.url= jdbc:mysql://localhost:3306/testdb?useSSL=false

# JWT
# JWT secret key
bezkoder.app.jwtSecret= bezKoderSecretKey
# Number of minutes after which an access token expires
bezkoder.app.jwtExpirationMs= 3600000
# Number of days after which a refresh token expires
bezkoder.app.jwtRefreshExpirationMs= 86400000

# SMTP configuration options for the email service
# For testing, you can use a fake SMTP service like Ethereal: https://ethereal.email/create
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=sminzhz@gmail.com
SMTP_PASSWORD=Abc@12345678
EMAIL_FROM=sminzhz@gmail.com
```

## Project Structure

```
src\main\resources\
 |--\application.properties    # Environment variables and configuration related things

src\java\com\bezkoder\spring\security\jwt\
 |--controllers\                                                     # Route controllers (controller layer)
 |--\docs\                                                           # Swagger files
 |--\advice\                                                         # Custom spring middlewares
 |--\models\                                                         # MySQL models (data layer)
 |--\controllers\AuthController.java/@RequestMapping("/api/auth")    # Routes
 |--\repository\                                                     # Business logic (service layer)
 |--\exception\                                                      # Utility classes and functions
 |--\payload\request                                                 # Request data validation schemas
 |--\payload\response                                                # Response data validation schemas
 |--\security\SpringBootSecurityJwtApplication.java                  # Spring app
 |--\security\SpringBootSecurityJwtApplication.java                  # App entry point
```

## API Documentation

To view the list of available APIs and their specifications, run the server and go to `http://localhost:3001/express/info/routes/` in your browser. This documentation page is automatically generated using the [swagger](https://swagger.io/) definitions written as comments in the route files.

### API Endpoints

List of available routes:

**Auth routes**:\
`POST /api/auth/register` - register\
`POST /api/auth/login` - login\
`POST /api/auth/logout` - logout\
`POST /api/auth/refresh-tokens` - refresh auth tokens\

**User routes**:\
`GET /api/check/heath` - get status app heath if return string "I'm Heath."\
`GET /api/check/user` - get user USER_MODERATOR\
`GET /api/check/moderator` - get user ROLE_MODERATOR\
`GET /api/check/admin` - get user ADMIN_MODERATOR\

## Error Handling

The app has a centralized error handling mechanism.

Controllers should try to catch the errors and forward them to the error handling middleware (by calling `next(error)`). For convenience, you can also wrap the controller inside the catchAsync utility wrapper, which forwards the error.

```javascript
const catchAsync = require('../utils/catchAsync');

const controller = catchAsync(async (req, res) => {
  // this error will be forwarded to the error handling middleware
  throw new Error('Something wrong happened');
});
```

The error handling middleware sends an error response, which has the following format:

```json
{
  "code": 404,
  "message": "Not found"
}
```

When running in development mode, the error response also contains the error stack.

The app has a utility ApiError class to which you can attach a response code and a message, and then throw it from anywhere (catchAsync will catch it).

For example, if you are trying to get a user from the DB who is not found, and you want to send a 404 error, the code should look something like:

```javascript
const httpStatus = require('http-status');
const ApiError = require('../utils/ApiError');
const User = require('../models/User');

const getUser = async (userId) => {
  const user = await User.findById(userId);
  if (!user) {
    throw new ApiError(httpStatus.NOT_FOUND, 'User not found');
  }
};
```

## Validation

Request data is validated using [Joi](https://joi.dev/). Check the [documentation](https://joi.dev/api/) for more details on how to write Joi validation schemas.

The validation schemas are defined in the `src/validations` directory and are used in the routes by providing them as parameters to the `validate` middleware.

```javascript
const express = require('express');
const validate = require('../../middlewares/validate');
const userValidation = require('../../validations/user.validation');
const userController = require('../../controllers/user.controller');

const router = express.Router();

router.post('/users', validate(userValidation.createUser), userController.createUser);
```

## Authentication

To require authentication for certain routes, you can use the `auth` middleware.

```javascript
const express = require('express');
const auth = require('../../middlewares/auth');
const userController = require('../../controllers/user.controller');

const router = express.Router();

router.post('/users', auth(), userController.createUser);
```

These routes require a valid JWT access token in the Authorization request header using the Bearer schema. If the request does not contain a valid access token, an Unauthorized (401) error is thrown.

**Generating Access Tokens**:

An access token can be generated by making a successful call to the register (`POST /v1/auth/register`) or login (`POST /v1/auth/login`) endpoints. The response of these endpoints also contains refresh tokens (explained below).

An access token is valid for 30 minutes. You can modify this expiration time by changing the `JWT_ACCESS_EXPIRATION_MINUTES` environment variable in the .env file.

**Refreshing Access Tokens**:

After the access token expires, a new access token can be generated, by making a call to the refresh token endpoint (`POST /v1/auth/refresh-tokens`) and sending along a valid refresh token in the request body. This call returns a new access token and a new refresh token.

A refresh token is valid for 30 days. You can modify this expiration time by changing the `JWT_REFRESH_EXPIRATION_DAYS` environment variable in the .env file.

## Authorization

The `auth` middleware can also be used to require certain rights/permissions to access a route.

```javascript
const express = require('express');
const auth = require('../../middlewares/auth');
const userController = require('../../controllers/user.controller');

const router = express.Router();

router.post('/users', auth('manageUsers'), userController.createUser);
```

In the example above, an authenticated user can access this route only if that user has the `manageUsers` permission.

The permissions are role-based. You can view the permissions/rights of each role in the `src/config/roles.js` file.

If the user making the request does not have the required permissions to access this route, a Forbidden (403) error is thrown.

## Logging

Import the logger from `src/config/logger.js`. It is using the [Winston](https://github.com/winstonjs/winston) logging library.

Logging should be done according to the following severity levels (ascending order from most important to least important):

```javascript
const logger = require('<path to src>/config/logger');

logger.error('message'); // level 0
logger.warn('message'); // level 1
logger.info('message'); // level 2
logger.http('message'); // level 3
logger.verbose('message'); // level 4
logger.debug('message'); // level 5
```

In development mode, log messages of all severity levels will be printed to the console.

In production mode, only `info`, `warn`, and `error` logs will be printed to the console.\
It is up to the server (or process manager) to actually read them from the console and store them in log files.\
This app uses pm2 in production mode, which is already configured to store the logs in log files.

Note: API request information (request url, response code, timestamp, etc.) are also automatically logged (using [morgan](https://github.com/expressjs/morgan)).

## Custom Mongoose Plugins

The app also contains 2 custom mongoose plugins that you can attach to any mongoose model schema. You can find the plugins in `src/models/plugins`.

```javascript
const mongoose = require('mongoose');
const { toJSON, paginate } = require('./plugins');

const userSchema = mongoose.Schema(
  {
    /* schema definition here */
  },
  { timestamps: true }
);

userSchema.plugin(toJSON);
userSchema.plugin(paginate);

const User = mongoose.model('User', userSchema);
```

### toJSON

The toJSON plugin applies the following changes in the toJSON transform call:

- removes \_\_v, createdAt, updatedAt, and any schema path that has private: true
- replaces \_id with id

### paginate

The paginate plugin adds the `paginate` static method to the mongoose schema.

Adding this plugin to the `User` model schema will allow you to do the following:

```javascript
const queryUsers = async (filter, options) => {
  const users = await User.paginate(filter, options);
  return users;
};
```

The `filter` param is a regular mongo filter.

The `options` param can have the following (optional) fields:

```javascript
const options = {
  sortBy: 'name:desc', // sort order
  limit: 5, // maximum results per page
  page: 2, // page number
};
```

The plugin also supports sorting by multiple criteria (separated by a comma): `sortBy: name:desc,role:asc`

The `paginate` method returns a Promise, which fulfills with an object having the following properties:

```json
{
  "results": [],
  "page": 2,
  "limit": 5,
  "totalPages": 10,
  "totalResults": 48
}
```

## Linting

Linting is done using [ESLint](https://eslint.org/) and [Prettier](https://prettier.io).

In this app, ESLint is configured to follow the [Airbnb JavaScript style guide](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base) with some modifications. It also extends [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) to turn off all rules that are unnecessary or might conflict with Prettier.

To modify the ESLint configuration, update the `.eslintrc.json` file. To modify the Prettier configuration, update the `.prettierrc.json` file.

To prevent a certain file or directory from being linted, add it to `.eslintignore` and `.prettierignore`.

To maintain a consistent coding style across different IDEs, the project contains `.editorconfig`

## Contributing

Contributions are more than welcome! Please check out the [contributing guide](CONTRIBUTING.md).

## Inspirations

- [danielfsousa/express-rest-es2017-boilerplate](https://github.com/danielfsousa/express-rest-es2017-boilerplate)
- [madhums/node-express-mongoose](https://github.com/madhums/node-express-mongoose)
- [kunalkapadia/express-mongoose-es6-rest-api](https://github.com/kunalkapadia/express-mongoose-es6-rest-api)

## License

[MIT](LICENSE)

# Database Descriptions

```
sudo mysql -u root -p
[sudo] password for ndmanh: 
Sorry, try again.
[sudo] password for ndmanh: 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 35
Server version: 8.0.29-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW TABLES;
ERROR 1046 (3D000): No database selected
mysql> USE testdb
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+--------------------+
| Tables_in_testdb   |
+--------------------+
| hibernate_sequence |
| refreshtoken       |
| roles              |
| user_roles         |
| users              |
+--------------------+
5 rows in set (0,00 sec)

mysql> INSERT INTO roles(name) VALUES('ROLE_USER');
Query OK, 1 row affected (0,00 sec)

mysql> INSERT INTO roles(name) VALUES('ROLE_MODERATOR');
Query OK, 1 row affected (0,01 sec)

mysql> INSERT INTO roles(name) VALUES('ROLE_ADMIN');
Query OK, 1 row affected (0,00 sec)

mysql> SHOW TABLES;
+--------------------+
| Tables_in_testdb   |
+--------------------+
| hibernate_sequence |
| refreshtoken       |
| roles              |
| user_roles         |
| users              |
+--------------------+
5 rows in set (0,01 sec)

mysql> SELECT * FROM users;
Empty set (0,00 sec)

mysql> SHOW COLUMNS FROM users;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint       | NO   | PRI | NULL    | auto_increment |
| email    | varchar(50)  | YES  | UNI | NULL    |                |
| password | varchar(120) | YES  |     | NULL    |                |
| username | varchar(20)  | YES  | UNI | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
4 rows in set (0,00 sec)

mysql> SHOW COLUMNS FROM user_roles;
+---------+--------+------+-----+---------+-------+
| Field   | Type   | Null | Key | Default | Extra |
+---------+--------+------+-----+---------+-------+
| user_id | bigint | NO   | PRI | NULL    |       |
| role_id | int    | NO   | PRI | NULL    |       |
+---------+--------+------+-----+---------+-------+
2 rows in set (0,00 sec)

mysql> SHOW COLUMNS FROM roles;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int         | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | YES  |     | NULL    |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0,01 sec)

mysql> SHOW COLUMNS FROM refreshtoken;
+-------------+--------------+------+-----+---------+-------+
| Field       | Type         | Null | Key | Default | Extra |
+-------------+--------------+------+-----+---------+-------+
| id          | bigint       | NO   | PRI | NULL    |       |
| expiry_date | datetime     | NO   |     | NULL    |       |
| token       | varchar(255) | NO   | UNI | NULL    |       |
| user_id     | bigint       | YES  | MUL | NULL    |       |
+-------------+--------------+------+-----+---------+-------+
4 rows in set (0,00 sec)

mysql> SHOW COLUMNS FROM hibernate_sequence;
+----------+--------+------+-----+---------+-------+
| Field    | Type   | Null | Key | Default | Extra |
+----------+--------+------+-----+---------+-------+
| next_val | bigint | YES  |     | NULL    |       |
+----------+--------+------+-----+---------+-------+
1 row in set (0,00 sec)

mysql> INSERT INTO users (email, password, username) VALUES ('sminzhz@gmail.com', '$2a$08$HDhm6omcDhTvyh8GqonTzu8QhndjAgBml.8bdnLUGn.OaT9RVd6F.', 'sminzhz@gmail.com');
mysql> INSERT INTO user_roles (user_id, role_id) VALUES (1, 3);
```

## Run Spring Boot application
```
mvn spring-boot:run
```

## Run following SQL insert statements
```
INSERT INTO roles(name) VALUES('ROLE_USER');
INSERT INTO roles(name) VALUES('ROLE_MODERATOR');
INSERT INTO roles(name) VALUES('ROLE_ADMIN');

mysql> SELECT * FROM users;
+----+-------------------+--------------------------------------------------------------+-------------------+
| id | email             | password                                                     | username          |
+----+-------------------+--------------------------------------------------------------+-------------------+
|  1 | sminzhz@gmail.com | $2a$08$HDhm6omcDhTvyh8GqonTzu8QhndjAgBml.8bdnLUGn.OaT9RVd6F. | sminzhz@gmail.com |
+----+-------------------+--------------------------------------------------------------+-------------------+
1 row in set (0,00 sec)

mysql> SELECT * FROM role_users;
ERROR 1146 (42S02): Table 'testdb.role_users' doesn't exist
mysql> SELECT * FROM user_roles;
+---------+---------+
| user_id | role_id |
+---------+---------+
|       1 |       3 |
+---------+---------+
1 row in set (0,00 sec)

mysql> SELECT * FROM roles;
+----+----------------+
| id | name           |
+----+----------------+
|  1 | ROLE_USER      |
|  2 | ROLE_MODERATOR |
|  3 | ROLE_ADMIN     |
+----+----------------+
3 rows in set (0,00 sec)

mysql> 
```

```
điều kì lạ
POST http://localhost:8080/api/auth/signin với payload
{
  "username": "sminzhz@gmail.com",
  "email": "sminzhz@gmail.com",
  "role": "ROLE_ADMIN",
  "password": "Abc@12345678"
}
sẽ nhận được response của http://localhost:8080/api/auth/signin mà đáng ra là phải với payload
{
  "email": "sminzhz@gmail.com",
  "password": "Abc@12345678"
}
```
