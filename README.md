# Swagger UI Koa 2

Forked from [swagger-ui-express](https://github.com/scottie1984/swagger-ui-express)

Adds middleware to your koa app to serve the Swagger UI bound to your Swagger document. This acts as living documentation for your API hosted from within your app.

Updated to Swagger 3.0.17

## Usage

In app's `package.json`

    "swagger-ui-koa": "latest" // or desired version

Setup `swagger-app-wrapper.js`
```javascript
import swaggerUi from 'swagger-ui-koa';
import swaggerJSDoc from 'swagger-jsdoc';
import convert from 'koa-convert';
import mount from 'koa-mount';
//import swaggerDocument from './swagger.json'; //also can be used

export default function (app) {
  //without jsdoc from swagger.json
  //app.use(swaggerUi.serve); //serve swagger static files
  //app.use(convert(mount('/swagger', swaggerUi.setup(swaggerDocument)))); //mount endpoint for access

  //with jsdoc
  const options = {
    swaggerDefinition: {
      info: {
        title: 'API', // Title (required)
        version: '2.0.0', // Version (required)
      },
    },
    apis: [
      './src/module/swagger/swagger.yaml',
      './src/routes/*.js', // Path to the API docs from root
      './src/module/swagger/parameters.yaml'
    ],
  };
  // Initialize swagger-jsdoc -> returns validated swagger spec in json format
  const swaggerSpec = swaggerJSDoc(options);
  app.use(swaggerUi.serve); //serve swagger static files
  app.use(convert(mount('/swagger', swaggerUi.setup(swaggerSpec)))); //mount endpoint for access
}

```

Open http://`<app_host>`:`<app_port>`/swagger in your browser to view the documentation.

### [swagger-jsdoc](https://www.npmjs.com/package/swagger-jsdoc)

### Swagger Explorer

By default the Swagger Explorer bar is hidden, to display it pass true as the second parameter to the setup function:

```javascript
const express = require('express');
const app = express();
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');

var showExplorer = true;
  ...
  swaggerUi.setup(swaggerDocument, showExplorer)
  ...
```

### Custom swagger options

To pass custom options e.g. validatorUrl, to the SwaggerUi client pass an object as the third parameter:

```javascript

var showExplorer = true;
var options = {
	validatorUrl : null
};
...
swaggerUi.setup(swaggerDocument, showExplorer, options));
...
```

### Custom CSS styles

To customize the style of the swagger page, you can pass custom CSS as the fourth parameter.

E.g. to hide the swagger header:

```javascript
var showExplorer = false;
var options = {};
var customCss = '#header { display: none }';

...
swaggerUi.setup(swaggerDocument, showExplorer, options, customCss));
...
```

## Requirements
* Koa 2
