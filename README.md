# React.js-Babel-Webpack
How to create an app with React.js, Babel and Webpack

```C
npm init -y
npm i -S react react-dom
```

Add ```react-native``` if we are building a mobile app.

## Babel
Babel is used to convert ES5 & ES6 code, so that it's backwards compatibale with older browsers.

### Packages
1. ***@babel/core:*** The main engine of babel
2. ***@babel/preset-env:*** Support for ES5 and ES6
3. ***@babel/preset-react:*** Support to use babel with react
4. ***babel-loader:*** The communication link between Webpack and Babel
```C
npm i -D @babel/core babel-loader @babel/preset-env @babel/preset-react
```

### Configure Babel
Create ```.babelrc``` and add:
```
{
  "presets":[
    "@babel/preset-env", 
    "@babel/preset-react",
  ]
}
```


## Webpack

### Packages
1. ***webpack:*** The main engine
2. ***webpack-cli:*** Adds the ability to access some webpack commands like starting the dev server, creating a development/production build, etc...
3. ***webpack-dev-server:*** Server for development
4. ***html-webpack-plugin:*** Adds the abiltiy to create HTML templates
```C
npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin
```

### Configure Webpack
Create ```webpack.config.js``` and add:
```
const path = require('path');
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
    context: __dirname,
    entry: path.join(__dirname, './src/javascript/index.js'),
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                use: {
                    loader: "babel-loader"
                }
            }
        ]
    },
    plugins: [
        new HtmlWebPackPlugin ({
            template: './src/html/index.html',
            filename: "./index.html",
        })
    ]
};
```


### package.json
```
{
 “name”: “client”,
 “version”: “1.0.0”,
 “description”: “”,
 “main”: “/javascript/index.js”,
 “scripts”: {
    "start": "webpack-dev-server --open --mode=development --hot",
    "dev": "webpack --mode=development --config webpack.config.js",
    "prod": "webpack --mode=production --config webpack.config.js"
 },
 “author”: “”,
 “license”: “ISC”,
 “dependencies”: {
   **“react”: “¹⁶.4.1”,**
   “react-dom”: “¹⁶.4.1”
 },
 “devDependencies”: {
   “@babel/core”: “^7.6.4”,
   “babel-loader”: “^8.0.6”,
   “@babel/preset-env”: “^7.6.3”,
   “@babel/preset-react”: “^7.6.3”,
   “html-webpack-plugin”: “³.2.0”,
   “webpack”: “⁴.41.2”,
   “webpack-cli”: “³.3.9”,
   “webpack-dev-server”: “³.9.0”
 }
}
```


## /src/html/index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>React app</title>
</head>
<body>
<div id="root"></div>
</body>
</html>
```

## /src/javascript/components/container/Hello.jsx
```
import React, {Component} from 'react';

class Hello extends Component {
    render() {
        return (
            <div>
                <h1>Hello World!</h1>
            </div>
        );
    }
}

export default Hello;
```

## /src/javascript/App.js
```
import React, {Component} from "react";
import Hello from "./components/container/Hello.jsx";

class App extends Component {

    render() {
        return(<Hello />
        );
    }
}

export default App;
```

## /src/javascript/index.js
```
import React from 'react';
import ReactDOM from 'react-dom';

import App from "./App";

function run() {
    ReactDOM.render(<App/>, document.getElementById('root'));
}

const loadedStates = ['complete', 'loaded', 'interactive'];

if (loadedStates.includes(document.readyState) && document.body) {
    run();
} else {
    window.addEventListener('DOMContentLoaded', run, false);
}
```
