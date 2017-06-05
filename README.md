# Nimiq 区块链 [![Build Status](https://travis-ci.com/nimiq-network/core.svg?token=euFrib9MJMN33MCBswws&branch=master)](https://travis-ci.com/nimiq-network/core) 

**[Nimiq](https://nimiq.com/)** 是第一个以浏览器为基础的区块链

## 快速入门 

1. 克隆此仓库 `git clone git@github.com:nimiq-network/core.git`.
2. 执行 `npm install`
3. 执行 `./node_modules/.bin/gulp build`
4. 在浏览器开启 `clients/browser/index.html` 以访问浏览器端

## 网页开发者
### 网页开发者的安装
参照快速入门

### 执行客户端

#### 执行浏览器端
在浏览器开启 `clients/browser/index.html`

#### 执行 NodeJs 端

要执行 NodeJs 端，你需要一个**公用可路由的IP位址**、**域名**和**SSL证书**（可从 [letsencrypt.org](https://letsencrypt.org/) 取得免费SSL证书，並执行 `clients/nodejs/index.js` 以开启NodeJs端。

```bash
cd clients/nodejs/
node index.js --host <hostname> --port <port> --key <privkey> --cert <certificate>
```

| 参数        | 说明           | 
| ------------- |:-------------:| 
| **_host_** | NodeJs端的主机名 | 
| **_port_** | 用于通信的端口 |  
| **_key_** | 客户端的私钥      | 
| **_cert_** | 域名的SSL证书       | 


### 建立自己的浏览器端
只要在项目中加入 `<script src="dist/nimiq.js"></script>`

### API 
访问 [API 文档](dist/API_DOCUMENTATION.md)


## 核心开发者
### 核心开发者的安装
- 最新版的 NodeJs (> 7.9.0)
- gulp：`npm install gulp -g`
- jasmine 测试框架：`npm install jasmine -g`
- 模块依赖：`npm install`
- NodeJs 模块依赖：

	```bash
	cd src/main/platform/nodejs/
	npm install
	cd clients/nodejs/
	npm install
	```

### 测试与建构

#### 执行测试集
- 此命令能在浏览器执行测试集 `gulp test`
- 此命令能在NodeJs执行测试集 `jasmine`

#### 执行 ESLint
此命令能执行 ESLint JavaScript 代码检查器 `gulp eslint`

#### 建构
执行命令 `gulp build`，将所有源码合并到 `dist/{web,web-babel,web-crypto,node}.js`

## 贡献

若你想贡献本项目，请遵循我们的 [行为守则](/.github/CONDUCT.md) 和 [贡献指引](/.github/CONTRIBUTING.md)

## 许可

本项目根据 [Apache 2.0 许可](./LICENSE) 授权您使用
