# Set up react app với webpack 4 và babel 7
> Trong bài viết này tôi sử dụng yarn package manager  
## Tạo project
```
mkdir webpack-babel-react
cd webpack-babel-react
yarn init -y
```  
## Cài đặt webpack
Tại thư mục root của project chạy lệnh:  
`yarn add webpack --dev`  
`yarn add webpack-cli --dev`  

## Set up babel
Babel là một công cụ dùng để chuyển các dòng lệnh được viết bằng cú pháp ES6, ES7 ra ES5 để trình duyệt có thể hiểu được  
Ta cần cài các dependencies sau:  
`yarn add @babel/core @babel/preset-env @babel/preset-react babel-loader --dev`  
Ta cũng phải tạo file `.babelrc` ở thư mục `root` để config cho babel
```
{
  "presets": ["@babel/env", "@babel/react"]
}
```  

## Config webpack
Tạo file `webpack.config.js` ở thư mục gốc nội dung như sau:  
```
module.exports = {
  module: {
    rules: [
      {
        test: /.(js|jsx)$/, // Là chuỗi regex, khi phân tích module thì webpack sẽ chỉ khớp với định dạng này
        exclude: /node_modules/, // Nơi khai báo bỏ qua các thư hoặc các file
        use: {
          loader: "babel-loader" // Dùng babel để chuyển code es6, es7 -> es5
        }
      }
    ]
  }
}
```  
Trong đó: 
`module` là nơi khai báo `loader`, `preloader`, `postloader`  
`loader` là phần khai báo quan trọng nhất chẳng hạn như trên là ta dùng `babel-loader` để compile các file có đuôi js hoặc jsx  
Còn `preloader` và `postloader` các bạn có thể tìm hiểu thêm trên trang chủ quả webpack  
## Code splitting trong webpack
[Chi tiết tại đây](https://kipalog.com/posts/Webpack-series--ep3----code-splitting---chia-code-trong-webpack)  
##Setup react
Ta cần cài một số depenencies sau:  
`yarn add react react-dom`  
Sau đó viết các component trong thư mục `src`  
## Kết nối các component với DOM 
`yarn add html-webpack-plugin --dev`  
Và chỉnh sửa file `webpack.config.js` như sau:  
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
  output: {
    path: path.join(__dirname, "build"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: path.join(__dirname, "src", "index.html")
    })
  ]
};
```  
Trong thư mục `src` tạo file `index.html`  
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>React, Webpack, Babel Starter Pack</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>
  <body>
    <noscript> You need to enable JavaScript to run this app. </noscript>
    <!-- your react app will render here -->
    <div id="app"></div>
  </body>
</html>
```  
### Thêm css loader
Cài các dependencies:  
`yarn add css-loader sass-loader mini-css-extract-plugin node-sass --dev`  

Chỉnh sửa lại file `webpack.config.js`  
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
  output: {
    path: path.join(__dirname, "build"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /.(css|scss)$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: path.join(__dirname, "src", "index.html")
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ]
};
```
## Cài webpack dev server
`yarn add webpack-dev-server --dev`  
Trong file `pakage.json` ta thêm đoạn sau:  
```
"scripts": {
    "start": "webpack --mode development",
    "dev": "webpack-dev-server --mode development --open",
    "build": "webpack --mode production"
  }
```  
Sau đó chạy  
`yarn run dev` để chạy môi trường `dev-server`  
`yarn run build` để build app  

