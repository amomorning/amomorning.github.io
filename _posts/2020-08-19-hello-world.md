---
layout: post
title: Hello World
date: 2020-08-18 22:26:08
tags: 
- daily
- web
- nginx
- javascript
  
description: a quick run down on accessing github metadata.
categories:
- 技术随笔
---

上半年来用云服务器搭了一些小应用，不知不觉发现可以比较完整地做一些东西了。顺手用 NginX 和 Hexo 配置了这个博客，版本控制放在 [Github](https://github.com/amomorning/blog) 上。

### NginX 的配置
Hexo 似乎是需要每次写完重新生成静态文件再部署，感觉稍微有些麻烦。如果用 `hexo server` 命令则可以动态看到博客内容的更新，所以为了省事就用 NginX 连到 Hexo 的端口了，直接在后台运行 `hexo s` 就可以看到博客了。
``` javascript
    server {
        listen 80;
        server_name blog.amomorning.com;
        location / {
            proxy_pass http://127.0.0.1:4000;
        }
        location /webhook {
            proxy_pass http://127.0.0.1:40067;
        }
    }
```
### pm2 监控博客服务器进程
为了服务器能稳定运行，用 `pm2` 守护进程监控服务，在服务挂掉的时候能自动重启。
``` javascript
// hexo-auto.js
var exec = require('child_process').exec;
var cmd = 'nohup hexo server >> server.log &';

exec(cmd, function(error, stdout, stderr) {
  process.exit(0);
});

```
用pm2运行该代码：
``` bash
pm2 start hexo-auto.js
```
### Webhook 侦听 git push
将博客内容放在 GitHub 的代码仓库之后，就可以在不同设备上写博客啦（希望真的会有更新哈哈哈  
因为懒于每次写完博客要连接服务器，就在用了 GitHub 设置里的 Webhook 功能，服务器收到代码仓库更新后能自动 `git pull`，更新博客内容。

``` javascript
// webhook.js
var http = require('http')
var createHandler = require('github-webhook-handler')
var handler = createHandler({path: '/webhook', secret: 'your webhook secret'})
const {exec} = require('child_process') // 执行本地命令

http.createServer(function(req, res) {
	handler(req, res, function(err) {
		res.statusCode = 404
		res.end('no such location')
	})
}).listen('40067')

handler.on('error', function(err) {
	console.error('Error:', err.message)
})

handler.on('push', function(event) {
	console.log('Received a push event for %s to %s',
		event.payload.repository.name,
		event.payload.ref)
	exec('cd /your/blog/dir && git pull', (err, stdout, stderr) => {
		if (err) {
			console.log(err)
		}
		else {
			console.log('updated success')
		}
	})
})

```
同样用pm2运行该代码：
``` bash
pm2 start webhook.js
```
<del>做得很方便用的样子，就是不知道博客里放点什么好呢 >w<</del>
