# Webpack Starter Template

## Setup
```
npm install webpack webpack-cli --save-dev
```

## NPM Scripts
```json
"scripts": {
    "build": "webpack --config webpack.prod.js",
    "build:dev": "webpack --config webpack.config.js",
    "dev": "webpack serve --open"
},
```


## Development Dependencies `--save-dev`

| Name                                                                                   | Package                        | Production  |
| -------------                                                                          |:-------------:                 |   -----:    |
| [Webpack CLI](https://webpack.js.org/api/cli/)                                         |  webpack-cli                   |     --      |
| [Dev Server](https://webpack.js.org/configuration/dev-server/)                         |  webpack-dev-server            |     --      |
| [HTML Loader](https://webpack.js.org/loaders/html-loader/)                             |  html-loader                   |     --      |
| [HTML Plugin](https://webpack.js.org/plugins/html-webpack-plugin/)                     |  html-webpack-plugin           |     --      |
| [CSS Loader](https://webpack.js.org/loaders/css-loader/)                               |  css-loader                    |     --      |
| [Mini CSS Extact Plugin](https://webpack.js.org/plugins/mini-css-extract-plugin/)      |  mini-css-extract-plugin       |     --      |
| [Style Loader](https://webpack.js.org/loaders/style-loader/)                           |  style-loader                  |     --      |
| [File Loader](https://v4.webpack.js.org/loaders/file-loader/)                          |  file-loader                   |     --      |
| [Copy Plugin](https://webpack.js.org/plugins/copy-webpack-plugin/)                     |  copy-webpack-plugin           |     --      |
| [CSS Minimizer Plugin](https://webpack.js.org/plugins/css-minimizer-webpack-plugin/)   |  css-minimizer-webpack-plugin  |     yes     |
| [Terser Plugin](https://webpack.js.org/plugins/terser-webpack-plugin/)                 |  terser-webpack-plugin         |     yes     |
| [Babel Loader](https://webpack.js.org/loaders/babel-loader/)                           |  babel-loader @babel/core      |     yes     |
| [Babel Preset](https://babeljs.io/docs/en/babel-preset-env)                            |  @babel/preset-env             |     yes     |

### All in One setup
```
npm i webpack webpack-cli html-loader html-webpack-plugin webpack-dev-server css-loader style-loader mini-css-extract-plugin file-loader copy-webpack-plugin css-minimizer-webpack-plugin terser-webpack-plugin babel-loader @babel/core @babel/preset-env --save-dev
```

## Dev Config

```javascript
const HtmlWebPackPlugin    = require('html-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CopyPlugin           = require("copy-webpack-plugin");

module.exports = {

    mode: 'development',
    output:{
        clean: true
    },
    module: {
        rules: [
            {
                test: /\.html$/i,
                loader: 'html-loader',
                options: {
                    minimize: false,
                    sources: false,
                }
            },
            {
                test: /\.css$/i,
                exclude: /styles.css$/,
                use: [ 'style-loader', 'css-loader' ]
            },
            {
                test: /styles.css$/,
                use: [ MiniCssExtractPlugin.loader, 'css-loader' ]
            },
            {
                test: /\.(png|jpe?g|gif)$/i,
                use: [ { loader: 'file-loader' } ]
            }
        ]
    },
    plugins: [
        new HtmlWebPackPlugin({
            template: './src/index.html',
            filename: './index.html',
            // inject: 'body'
        }),
        new MiniCssExtractPlugin({
            filename: '[name].css',
            ignoreOrder: false
        }),
        new CopyPlugin({
            patterns: [
                { from: 'src/assets/', to: 'assets/' }
            ]
        })
    ]

}
```

## Prod Config
```javascript
const HtmlWebPackPlugin    = require('html-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CopyPlugin           = require("copy-webpack-plugin");

const CssMinimizer = require('css-minimizer-webpack-plugin');
const Terser = require('terser-webpack-plugin');

module.exports = {

    mode: 'production',
    output:{
        clean: true,
        filename: '[name].[contenthash].js'
    },
    module: {
        rules: [
            {
                test: /\.html$/i,
                loader: 'html-loader',
                options: {
                    minimize: false,
                    sources: false,
                }
            },
            {
                test: /\.css$/i,
                exclude: /styles.css$/,
                use: [ 'style-loader', 'css-loader' ]
            },
            {
                test: /styles.css$/,
                use: [ MiniCssExtractPlugin.loader, 'css-loader' ]
            },
            {
                test: /\.(png|jpe?g|gif)$/i,
                use: [ { loader: 'file-loader' } ]
            },
            {
                test: /\.m?js$/,
                exclude: /node_modules/,
                use: {
                  loader: "babel-loader",
                  options: {
                    presets: ['@babel/preset-env']
                  }
                }
              }
        ]
    },
    optimization: {
        minimize: true,
        minimizer: [
            new CssMinimizer(),
            new Terser(),
        ]
    },
    plugins: [
        new HtmlWebPackPlugin({
            template: './src/index.html',
            filename: './index.html',
            // inject: 'body'
        }),
        new MiniCssExtractPlugin({
            filename: '[name].[fullhash].css',
            ignoreOrder: false
        }),
        new CopyPlugin({
            patterns: [
                { from: 'src/assets/', to: 'assets/' }
            ]
        })
    ]

}
```
