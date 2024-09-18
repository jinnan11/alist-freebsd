## Serv00 部署 AList

实现真正的访问即保活，访问即唤醒。

实现重启进程自动更新最新版的Alist，运行 AList

---

### 部署 Alist 前的一些准备工作

**参考[使用Serv00免费虚拟主机部署Alist](https://zhuanlan.zhihu.com/p/680607217)**

1. 注册账号

2. 允许Run your own applications

3. 添加两个TCP端口

4. 新建一个数据库

**接着回到Panel中，找到WWW Websites选项卡**

使用Add website功能新建一个Node.js类型的网站

可删除原来的xxx.serv00.net并重新使用xxx.serv00.net

![新建Node.js类型的网站](https://github.com/user-attachments/assets/b8ef1f8b-edcd-4cee-9510-a4ca096de1b0)

最后点击下方的 Add 按钮进行生成。

**自定义域名**

(使用xxx.serv00.net二级域名可跳过该步骤)

在 Panel 中点击左侧菜单栏中的 SSL ，然后点击上方菜单栏中的 WWW websites ，复制第一个 IP Address ，并在网站管理处添加 A 记录

![自定义域名](https://github.com/user-attachments/assets/146db739-256a-4e52-b080-ad1c2e64896c)


**生成一个 Let's Encrypt 证书：**

在 Panel 中点击左侧菜单栏中的 SSL ，然后点击上方菜单栏中的 WWW websites ，点击第一个 IP Address 最右侧的 Manage 按钮，再点击上方菜单栏中的 Add certificate 按钮，Type 选择 Generate Let's Encrypt certificate， Domain任选一个即可，最后点击下方的 Add 按钮进行生成。

![Manage](https://github.com/user-attachments/assets/0e9f7dfe-dfe4-4bf7-9c6e-6c87ee0b0677)


![Add certificate](https://github.com/user-attachments/assets/48e74666-633a-4f29-a2cd-6363e78f3514)

---

### 部署Alist

**接着使用SSH登入到你的账户，我使用的SSH客户端是[Termius](https://termius.com)**

![SSH](https://github.com/user-attachments/assets/8b53ddc3-123f-4d08-944c-3a3922473b75)


**进入 nodejs 的工作目录：**

~~~
cd ~/domains/网站/public_nodejs
~~~

记得把网站替换成你的网站

**下载AList和运行Node.js的基础文件：**

~~~
bash <(curl -s https://raw.githubusercontent.com/jinnan11/serv00-api/main/install_alist.sh)
~~~

![SSH](https://github.com/user-attachments/assets/96fc3313-955b-41e3-babf-986096fca470)


**根据提示修改相关端口号。**

右键点击文件，选择View/Edit > Source Editor，进行编辑

首次使用需要在Choose another editor添加Source Editor

![Source Editor](https://github.com/user-attachments/assets/5de75781-0cd9-420d-a398-12b020139aeb)


1. app.js 第13行端口号需要和data/config.json中的第26行的端口号保持一致

![app.js](https://github.com/user-attachments/assets/84089bb6-d0dd-4af1-abcc-6ea60c108f4d)


2. 修改data/config.json

   参考[使用Serv00免费虚拟主机部署Alist](https://zhuanlan.zhihu.com/p/680607217)

   补充

   第25行的IP地址无需修改，使用默认0.0.0.0

   第26行的端口号需要和app.js中的第13行端口号保持一致

   第83行的端口号5246改为0

![data/config.json](https://github.com/user-attachments/assets/465139e4-fae6-4293-8a8a-5c403d279eff)


**启动一次AList，查看运行是否正常**

~~~
./web.js server
~~~

![启动AList](https://github.com/user-attachments/assets/e2b04370-7506-4c12-9a8f-1f02ab99bfcf)


运行正常，记得把管理员用户的密码记住。接着使用Ctrl+c停止运行。

**安装npm22:**

~~~
npm22 install
~~~

![安装npm22](https://github.com/user-attachments/assets/2054252d-c406-41de-ab9c-30aff1fb11ae)


访问您的网站

![槿南盘](https://github.com/user-attachments/assets/ab6b52dd-fb96-497e-a7a6-ab87b0b996b1)


---

### 自动启动

你可以通过访问网站对项目进行唤醒，如果你需要保活，可以使用以下公共服务对网页进行监控：

1. [cron-job.org](https://console.cron-job.org)
2. [UptimeRobot](https://uptimerobot.com/) 

同时，你也可以选择自建 [Uptime-Kuma](https://github.com/louislam/uptime-kuma) 等服务进行监控。

---

### 常见问题

1. **如何更新AList**

   SSH 登录 Serv00，输入以下命令：
   
   ~~~
   killall -u $(whoami)
   ~~~

   访问网站

2. **国内为什么访问不了网站**

   Serv00部分服务器不定期屏蔽国内IP。

   套一层CF 或 使用代理工具，访问网站

4. **为什么挂载不了国内网盘**

   Serv00部分服务器不定期屏蔽国内IP。

   看邮件 SSH/SFTP server address

   ~~~
   s7.serv00.com = S7
   s8.serv00.com = S8
   ~~~

   请自行访问测试站查看支持情况：
   
   测试站：https://test.jnpan.top

---

### 本教程常见指令

**随机生成一个AList密码**

~~~
./web.js admin random
~~~

(需要cd ~/domains/网站/public_nodejs路经下运行)

**关闭用户所有进程**

~~~
killall -u $(whoami)
~~~

---

### 相关参考网站

使用Serv00免费虚拟主机部署Alist：https://zhuanlan.zhihu.com/p/680607217

Serv00 进程保活最终解决方案：https://saika.us.kg/2024/08/15/serv00-keep-alive
