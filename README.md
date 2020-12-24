
# 按需加载,自定义主题。原文地址:
```
https://blog.csdn.net/qq_39223195/article/details/106287522
```
各种自定义主题，啥配置可以参考官网文档:

```
https://ant.design/docs/react/use-with-create-react-app-cn
```
详情可参考有道笔记

------------------------------------------------------



### 配置步骤
1,首先，使用 create-react-app 创建一个项目，这里我们命名为 my-project

```
$ npx create-react-app my-project
```

2,进入项目目录，安装依赖

```
$ npm install antd -S
$ npm install @craco/craco craco-less @babel/plugin-proposal-decorators babel-plugin-import -D
```
3,根目录下创建 craco.config.js

```
const CracoLessPlugin = require('craco-less')
const path = require('path')

const pathResolve = pathUrl => path.join(__dirname, pathUrl)

module.exports = {
  webpack: {
    alias: {
      '@@': pathResolve('.'),
      '@': pathResolve('src'),
      '@assets': pathResolve('src/assets'),
      '@common': pathResolve('src/common'),
      '@components': pathResolve('src/components'),
      '@hooks': pathResolve('src/hooks'),
      '@pages': pathResolve('src/pages'),
      '@store': pathResolve('src/store'),
      '@utils': pathResolve('src/utils')
      // 此处是一个示例，实际可根据各自需求配置
    }
  },
  babel: {
    plugins: [
      ['import', { libraryName: 'antd', style: true }],
      ['@babel/plugin-proposal-decorators', { legacy: true }]
    ]
  },
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
      	// 此处根据 less-loader 版本的不同会有不同的配置，详见 less-loader 官方文档
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: {'@primary-color': 'green'},
            javascriptEnabled: true
          }
        }
      }
    }
  ]
}
```
4,修改 package.json 中的 scripts

```
{
  "scripts":{
    "start": "set PORT=5000 && craco start",
    "build": "set GENERATE_SOURCEMAP=false && craco build",
    "test": "craco test"
  }
}
```

5,上面用到了两个环境变量：

```
PORT 启动端口
GENERATE_SOURCEMAP 打包时是否生成 sourceMap

根目录下创建 jsconfig.json
```


```
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@@/*": ["./*"],
      "@/*": ["src/*"],
      "@assets/*": ["src/assets/*"],
      "@common/*": ["src/common/*"],
      "@components/*": ["src/components/*"],
      "@hooks/*": ["src/hooks/*"],
      "@pages/*": ["src/pages/*"],
      "@store/*": ["src/store/*"],
      "@utils/*": ["src/utils/*"]
    },
    "experimentalDecorators": true
  },
  "exclude": ["node_modules", "build"]
}
```
对应 craco.config.js 中配置的别名即可，其他更详细配置可以参考 vscode 官方文档。

至此，项目便配置完成。

-----------------------------
