[TOC]

## npm换源

* 使用cnpm

```json
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm -v
```

* 永久换源

```
npm config set registry https://registry.npm.taobao.org
方法二:
1.打开.npmrc文件（C:\Program Files\nodejs\node_modules\npm\npmrc，没有的话可以使用git命令行建一个( touch .npmrc)，用cmd命令建会报错）
2.增加 registry =https://registry.npm.taobao.org 
```

验证

```
npm config get registry
npm info express
```

还原

```
npm config set registry https://registry.npmjs.org/
```

