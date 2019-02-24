# Antd入门

## 安装

可以选择以下两种方式初始化一个ant design项目。

1. 直接 clone git 仓库（推荐）
```
git clone --depth=1 https://github.com/ant-design/ant-design-pro.git antd-demo
cd antd-demo
```

2. 使用命令行工具（不推荐，下载模板不成功）
```
$ npm install ant-design-pro-cli -g
$ mkdir antd-demo && cd antd-demo
$ pro new  # 安装脚手架
```

## 目录结构

```
├── config                   # 配置文件
│   └── config.js            # umi配置文件
│   └── plugin.config.js     # webpack插件配置
│   └── router.config.js     # **菜单（umi路由）配置**
├── docker                   # docker compose（暂时可忽略）
│   └── docker-compose.dev.yml   # 开发环境
│   └── docker-compose.yml       # 生产环境
│   └── nginx.conf               # nginx配置文件
├── mock                     # **本地模拟数据**
├── node_modules             # nodejs依赖插件
│   └── ...                  # 在运行npm install后会将依赖的插件下载到该目录
├── public
│   └── favicon.ico          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── common               # 应用公用配置，如导航信息（已废弃）
│   ├── components           # 业务通用组件
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # **通用布局**
│   ├── locales              # **国际化**
│   ├── models               # **dva model**（通用数据模型）
│   ├── pages                # **页面文件**
│   ├── routes               # 业务页面入口和常用模板（已废弃）
│   ├── services             # **后台接口服务**
│   ├── utils                # **工具库**
│   ├── app.js               # 
│   ├── defaultSettings.js   # **默认全局配置**
│   ├── global.js            # 
│   ├── global.less          #
│   ├── manifest.json        # 
│   └── service-worker.js    # 路由入口
├── tests                    # 测试工具
├── README.md                # 项目说明文档
└── package.json             # webpack配置文件
```

注意：以上目录或文件的说明中以**标注的是实际开发接触最多的，其它的目录或文件基本不需要做改动，因而可以暂时忽略。

## 本地开发

```
$ npm install（安装依赖插件）
$ npm start（启动服务器）
```