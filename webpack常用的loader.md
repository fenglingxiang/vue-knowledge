## webpack 常用的 loader

1. js 处理的 loader

- babel-loader
- ts-loader
- terser-webpack-plugin

```js
  {
    test: /\.js$/,
    exclude: /node_modules/,
    use: {
      loader: "babel-loader",
      options: {
        presets: ["@babel/preset-env"],
        plugins: [
          "@babel/plugin-transform-runtime"
        ],
        cacheDirectory: true //开启缓存
      }
    }
  },
  {
    test: /\.tsx?$/,
    use: [
      {
        loader: "ts-loader",
        options: {
          configFile: "tsconfig.json"
        }
      }
    ],
    exclude: /node_modules/
  }
```

```js
const TerserPlugin = require('terser-webpack-plugin');

optimization: {
  minimize: true,
  minimizer: [
    new TerserPlugin({
      parallel: true,                  // 并行压缩
      terserOptions: {
        compress: {
          drop_console: process.env.NODE_ENV === 'production', // 生产环境移除console
          pure_funcs: ['console.log']  // 移除指定函数
        },
        mangle: {
          safari10: true              // 修复Safari 10循环变量错误
        }
      },
      extractComments: false           // 不提取注释到单独文件
    })
  ]
}
```

2. 样式处理

- style-loader
- css-loader
- sass-loader
- postcss-loader
- css-minimizer-webpack-plugin（CSS 压缩）

```js
{
  test: /\.scss$/,
  use: [
    "style-loader",
    {
      loader: "css-loader",
      options: {
        modules: true, //启用css module
        importLoaders: 2 //在css-loader之前应用的loaders数
      }
    },
    "sass-loader", //将scss编译成css
    "postcss-loader", //自动添加浏览器前缀
    MiniCssExtractPlugin.loader,  // 生产环境提取CSS、压缩
  ]
},
```

```js
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

// webpack配置optimization中
optimization: {
  minimizer: [
    "...", // 继承默认的JS压缩器
    new CssMinimizerPlugin({
      minimizerOptions: {
        preset: [
          "default",
          {
            discardComments: { removeAll: true },
            cssDeclarationSorter: true,
          },
        ],
      },
    }),
  ];
}
```

3. 图片文字资源处理

- url-loader
- file-loader

```js
{
  test: /\.(jpg|png|jpeg|svg|gif)$/,
  use: [
    {
      loader: "url-loader",
      options: {
        limit: 10240, //小于10kb的图片转成base64,
        name: "images/[name].[hash:8].[ext]", //图片输出路径
        fallback: "file-loader" //超出limit的图片使用file-loader处理
      }
    },
    "image-webpack-loader" //压缩图片
  ]
},
{
  test: /\.(woff|woff2|eot|ttf|otf)$/,
  use: [
    {
      loader: "file-loader",
      options: {
        name: "fonts/[name].[hash:8].[ext]"
      }
    }
  ]
}
```
