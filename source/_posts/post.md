---
title: Next网易云跟帖,利用回推接口实现实时邮箱通知
date: 2017-05-09 13:58:41
categories:
  - Node
tags:
  - Node
  - hexo
---
最近很忙，很久没写东西，上周末上自己博客发现多说已经停了

虽然自己博客流量惨不忍睹，但是梦想还是要有的，万一红了呢，没有评论系统这种事，不可能的(马老师口气)

看next文档，果断升级5.1版本，开了网易云跟帖。

一顿配置，最后发现一个回推接口选项，不支持直接推邮箱。

正好最近想学node，就试试写了个简易服务器，接收回推，并发送邮件通知。

代码本身无难度，但是坑也很多，花了我一天才跑通

首先你要有一个服务器

1. 安装 `nodemailer`
    ```
    npm i nodemailer --save
    ```
    <!-- more -->
2. 新建一个js文件，比如`email.js`

    下面贴下代码

    ```js
    const nodemailer = require('nodemailer');
    const http = require('http')
    const queryString = require('querystring')


    //新建邮箱服务
    const transporter = nodemailer.createTransport({
        host: 'smtp.qq.com',
        port: 465,

        auth: {
            user: '***@qq.com',
            pass: '****'
        }
    });

    //创建一个http服务器，接收回退数据
    http.createServer(function (req, res) {
        var content = '';
        req.on('data', function (chunk) {
            content += chunk;
        })
        req.on('end', function () {
            res.writeHead(200, { 'Content-Type': 'text/plain' });
            res.write('re:' + content);
            res.end();
            //对数据进行url解码
            let text = queryString.parse(content).data;
            //转换为Object
            content = JSON.parse(text)[0]

            let mailOptions = {
                from: '"网易回帖 👻" <463428221@qq.com>', // 发送方
                to: 'dec_fan@icloud.com', // 接收邮箱，可多个。
                subject: 'Hello ✔', // 主题
                text: text, // 文本（string）
                html: `<h3>${content['title']}</h3><a href='${content['url']}'>原文链接</a><h4>昵称：${content['comments'][0].user.userId}</h4><p>内容：${content['comments'][0].content}</p>` // html 
            };
            transporter.sendMail(mailOptions, (error, info) => {
                if (error) {
                    return console.log(error);
                }
                console.log('Message %s sent: %s', info.messageId, info.response);
            });

        })

    }).listen(80)
    ```
3. 运行服务
```
//cd 到email.js 所在文件夹,以下命令都是在此文件夹下

$ node email.js
```

4. 使用云跟帖的回推测试
成功的话。会打印
```
Message sent:********** 250
```
5. 后台运行email.js
方法有好几种，我是个Linux柴鸡，我用的是`&`+`disown`命令
```
$ node email.js &

& disown
```

截图:

![](http://omqz8y2im.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170509130419.png)


代码很简单的，只是里面小坑太多，你可以自己填，或者直接复制粘贴

好了，继续搬砖去了。欢迎留言测试。哈哈哈
