---
title: 如何用js定时发送情话
categories:
- [秃头程序员, 奇怪的知识]
---

:::danger no-icon

~~淦，为什么我一个单身狗要学这种奇怪的知识~~

==老天快点赐我一个女朋友吧！==

:::

:::primary

准备工作

:::

[nodejs](https://nodejs.org/en/)

:::info

开始操作

:::

#### 创建一个新的文件夹[mailBot]{.label}

#### 打开[cmd]{.label}命令行

初始化[npm]{.label}并安装邮件发送模块

```
npm init -y
npm install nodemailer
```

#### 在项目中新建[index.js]{.label}文件，编写如下代码:

```
const nodemailer = require("nodemailer");
// 发送邮件函数
async function sendMail(text) {
  var user = "xxx@qq.com";//自己的邮箱
  var pass = "xxx"; //qq邮箱授权码,如何获取授权码下面有讲
  var to = "waitfor_1153@qq.com";//对方的邮箱
  let transporter = nodemailer.createTransport({
    host: "smtp.qq.com",
    port: 587,
    secure: false,
    auth: {
      user: user, // 用户账号
      pass: pass, //授权码,通过QQ获取
    },
  });
  let info = await transporter.sendMail({
    from: `sumerwind<${user}>`, // sender address
    to: `未来女朋友<${to}>`, // list of receivers
    subject: "做白日梦", // Subject line
    text: text, // plain text body
  });
  console.log("发送成功");
}
​
//测试一下
send('夏天的风')
​
```

[在终端中输入 `node index.js` 就可以执行js文件的代码]{.grey}

[qq邮箱的pass（授权码）需要进入 qq邮箱 的【设置】-【账户】,开启smtp]{.grey}

#### 自动生成情话

使用[彩虹屁生成网站接口](https://chp.shadiao.app/api.php)生成要发送的内容

使用[axios]{.label}模块下载生成的情话

```
npm i axios
```

在[index.js]{.label}中增加如下代码来使用[axios]{.label}获取情话

```
const { default: Axios } = require("axios");
function getHoneyedWords() {
  var url = "https://chp.shadiao.app/api.php";
  //获取这个接口的信息
  return Axios.get(url);
}
```

#### 用邮件发送情话

[index.js]{.label}中增加测试邮件发送代码：

```
//获取情话
getHoneyedWords().then(res=>{
    console.log(res.data)
  //发送邮件
    sendMail(res.data);
})
​
```

#### 终端输入

```
node index.js
```

[:heavy_check_mark:success]{.label .success}

#### 每天定时发送

使用[node-schedule]{.label}模块

```
npm install node-schedule
```

[index.js]{.label}增加如下代码

```
const schedule = require("node-schedule");
//每天下午5点21分发送
schedule.scheduleJob({ hour: 17, minute: 21 }, function () {
  console.log("启动任务:" + new Date());
  getHoneyedWords().then((res) => {
    console.log(res.data);
    sendMail(res.data);
  });
});
​
```

#### 终端输入：

```
node index.js
```

