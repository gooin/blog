---
title: npm 发布私有包
date: 2021-11-04
draft: false
description: ""
tags: ["frontend"]
---


今天发布一个内部的包，试了好久没有发布出来，每次发布运行`npm publish`的时候会报401错误。
```bash
npm notice === Tarball Details ===
npm notice name:          my-package
npm notice version:       0.0.1
npm notice package size:  190.2 kB
npm notice unpacked size: 220.0 kB
npm notice shasum:        e7e2da159ee6b150de95f57f9773561e449aef01
npm notice integrity:     sha512-yUdhJZjtOBu37[...]XQwm0otvFxaSw==
npm notice total files:   20
npm notice
npm ERR! code E401
npm ERR! 401 Unauthorized - PUT https://registry.npm.taobao.org/my-package - [unauthorized] Login first

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\gooin\AppData\Roaming\npm-cache\_logs\2021-11-04T03_42_26_692Z-debug.log

```
在 `package.json`中，明明是指定了`publishConfig`的。
```json
   "publishConfig": {
        "@xxx:registry": "http://192.168.198.162/api/v4/projects/627/packages/npm/"
    },
```
用`nrm`将源更换到npm后，在运行`npm publish` 发现将包发布到了npm仓库，这个时候注意到了包名称：`my-package`。
回头在看`publishConfig` ，是指定了`scope`的，也就是`@xxx`这个地方。

## 解决方案
修改package的名称为`@xxx/my-package`
```shell
form: 
"name": "my-package"
to:
"name": "@xxx/my-package"
```
然后在 `npm publish` 就行了。

## 注意事项
初始化空白项目的时候。如果在某个组织下。  应该运行 `npm init --scope=@xxx` 这样`package.json`中的`name` 就会补充上 `@xxx`组织名称了。
