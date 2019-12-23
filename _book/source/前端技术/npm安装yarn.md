# yarn使用

###  安装

```shell
npm install -g yarn --registry=https://registry.npm.taobao.org
```

### 配置yarn源

```shell
yarn config set registry https://registry.npm.taobao.org -g

yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
```

> -g 全局安装



## 使用说明

现在 Yarn 已经 [安装完毕](https://yarn.bootcss.com/docs/install)，可以开始使用了。 以下是一些你需要的最常用的命令：

### 初始化一个新项目

```shell
yarn init
```

### 添加依赖包

```shell
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

### 将依赖项添加到不同依赖项类别中

分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

```shell
yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional
```

### 升级依赖包

```shell
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

### 移除依赖包

```shell
yarn remove [package]
```

### 安装项目的全部依赖

```shell
yarn
```

或者

```shell
yarn install
```