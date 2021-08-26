# Debugging

## CORS

To run Chrome Dev with CORS check disabled:

```
open -n -a /Applications/Google\ Chrome\ Dev.app/Contents/MacOS/Google\ Chrome\ Dev --args --disable-web-security
```

## Create React App

To disable minification (temporarily!) for production build modify code inside `react-scripts` package: `node_modules/react-scripts/config/webpack.config.js`.

On line `243` change this bit:

```js
optimization: {
      minimize: false,
  // ...
```
