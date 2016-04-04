---
title: 谁也无法阻止我用上最新的JavaScript.md
tags:
---

## 事情的起因
最近在研究[reactStartKit](https://www.reactstarterkit.com/),
分享下我分析它启动的脑图[reactStartKit脑图](http://naotu.baidu.com/file/e4a57c78d4971c3c51d036c4bf096bcd?token=b164146c8ab5762e).
packge.json里的script start是这样的:
```javascript
"start": "babel-node --eval \"require('./tools/start')().catch(err => console.error(err.stack))\""
```
用的是babel-node,

