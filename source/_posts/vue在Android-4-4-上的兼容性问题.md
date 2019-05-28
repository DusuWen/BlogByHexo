---
title: vue在Android 4.4 上的兼容性问题
date: 2018-06-21 14:52:57
tags:
 - Vue
---

## 解决Vue在安卓4.4上的不兼容白屏问题

### 安装babel-polyfill

```javascript
npm install babel-polyfill
```

### main.js引入

```
import 'babel-polyfill'
```

注意，需要放在最顶端

### webpack.base.conf.js引入

修改entry为

```
entry: ['babel-polyfill','./src/main.js'],
```

重新build即可解决大部分白屏问题