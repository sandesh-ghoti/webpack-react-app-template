### npm init

    `npm init`

### Install dependacies

    `npm i react react-dom react-router-dom react-redux @reduxjs/toolkit`

### Install dev dependencies for tailwind

ref [using postcss](https://tailwindcss.com/docs/installation/using-postcss)
`npm install -D tailwindcss postcss autoprefixer`
`npx tailwindcss init`

### Install dev dependencies webpack

    `npm i -D webpack webpack-cli webpack-dev-server` // webpack-dev-server for live-server at localhost

    #### Plugins required for
        1. HTML
            `npm i -D html-webpack-plugin`
        2. copy assets from public directory which are required for index.html
        ` npm i -D copy-webpack-plugin`
    #### Loader required for
        1. CSS
            `npm i -D css-loader style-loader postcss-loader` //`sass-loader` use if you have scss files
        2. Assets
            configured on config.js file
        3. React
            `npm i -D @babel/preset-react`
        4. babel
            `npm i -D @babel/cli @babel/core @babel/plugin-transform-runtime @babel/runtime @babel/preset-env  babel-loader` //@babel/preset-env to load modules from node_modules

### add cmd in package.json

`"scripts": {
    "dev": "webpack serve --mode development",  //to run webpack live server
    "build": "webpack --mode production"  // to build production build
  },`

## webpack.config.js file

```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");

module.exports = {
  entry: {
    bundle: path.resolve(__dirname, "src/index.js"),
  },
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "[name][contenthash].js",
    clean: true, // remove old build files
    assetModuleFilename: "[name][ext]",
  },
  devtool: "source-map", // if we got any errors then it will us line no. of our non dist files line no.
  devServer: {
    static: {
      directory: path.resolve(__dirname, "dist"),
    },
    port: 3000,
    open: true, // after npm run dev it open url in browser
    hot: true, // turn on hot module replacement
    compress: true, // compression g-zip mode
    historyApiFallback: true,
  },
  module: {
    rules: [
      { // to bundle all css files. if you are using sass then add matchers here
        test: /\.css$/i,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env", "@babel/preset-react"],
          },
        },
      },
      { // load all assets are imported in any js file into asset/resource folder
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    new CopyWebpackPlugin({ //copy the assets from public folder
      patterns: [{ from: "public", to: "" }],
    }),
    new HtmlWebpackPlugin({ //generate index.html from template.html file
      title: "custom App",
      template: "./public/template.html",
    }),
  ],
};

```
