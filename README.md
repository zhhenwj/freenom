<div align="center">
<h1>Freenom：freenom域名自动续期</h1>

[![Build Status](https://img.shields.io/badge/build-passed-brightgreen?style=for-the-badge)](https://scrutinizer-ci.com/g/luolongfei/freenom/build-status/master)
[![Php Version](https://img.shields.io/badge/php-%3E=7.1-brightgreen.svg?style=for-the-badge)](https://secure.php.net/)
[![Scrutinizer Code Quality](https://img.shields.io/badge/scrutinizer-9.31-brightgreen?style=for-the-badge)](https://scrutinizer-ci.com/g/luolongfei/freenom/?branch=master)
[![MIT License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=for-the-badge)](https://github.com/luolongfei/freenom/blob/master/LICENSE)

Documentation: [English version](https://github.com/luolongfei/freenom/blob/master/README_EN.md) | 中文版
</div>

[📃 前言](#-前言)

[🍭 效果](#-效果)

[🎁 事前准备](#-事前准备)

[📪 配置发信邮箱](#-配置发信邮箱)

[🚧 配置脚本](#-配置脚本)

[🎈 添加计划任务](#-添加计划任务)

[☕ 验证](#-验证)

[🤣 本项目最简单的使用方法](#-本项目最简单的使用方法)

[🍺 信仰](#-信仰)

[❤ 捐赠 Donate](#-捐赠-donate)

[📋 捐赠名单 Donate List](#-捐赠名单-donate-list)

[🌚 作者](#-作者)

[🎉 鸣谢](#-鸣谢)

[🥝 开源协议](#-开源协议)


### 📃 前言
众所周知，Freenom是地球上唯一一个提供免费顶级域名的商家，不过需要每年续期，每次续期最多一年。由于我申请了一堆域名，而且不是同一时段申请的，
所以每次续期都觉得折腾，于是就写了这个自动续期的脚本。

### 🍭 效果
![邮件示例](https://s2.ax1x.com/2020/01/31/139Rrd.png "邮件内容")

无论是续期成败或者脚本执行出错，都会收到的程序发出的邮件。如果是续期成败相关的邮件，邮件会包括未续期域名的到期天数等内容。
邮件参考了微信发送的注销公众号的邮件样式。

### 🎁 事前准备
- 发信邮箱：为了方便理解又称机器人邮箱，用于发送通知邮件。目前支持`Gmail`、`QQ邮箱`以及`163邮箱`，程序会自动判断发信邮箱类型并使用合适的配置。推荐使用`Gmail`。
- 收信邮箱：用于接收机器人发出的通知邮件。推荐使用`QQ邮箱`，`QQ邮箱`唯一的好处只是收到邮件会在`QQ`弹出消息。
- VPS：随便一台服务器都行，系统推荐`Centos7`，另外PHP版本需在`php7.1`及以上。**（注：没有 VPS 也行，本项目支持在 Github Actions 上执行，完全白嫖，具体使用方法请参考「 [🤣本项目最简单的使用方法](#-本项目最简单的使用方法) 」）**
- 没有了

### 📪 配置发信邮箱
下面分别介绍`Gmail`、`QQ邮箱`以及`163邮箱`的设置，你只用看自己需要的部分。注意，`QQ邮箱`与`163邮箱`均使用账户加授权码的方式登录，
`谷歌邮箱`使用账户加密码的方式登录，请知悉。另外还想吐槽一下，国产邮箱你得花一毛钱给邮箱提供方发一条短信才能拿到授权码。

#### 设置Gmail
***
1、在`设置>转发和POP/IMAP`中，勾选
- 对所有邮件启用 POP 
- 启用 IMAP


![gmail配置01](https://s2.ax1x.com/2020/01/31/13tKsg.png "gmail配置01")

然后保存更改。

2、允许不够安全的应用

登录谷歌邮箱后，访问[谷歌权限设置界面](https://myaccount.google.com/u/0/lesssecureapps?pli=1&pageId=none)，启用允许不够安全的应用。

![gmail配置02](https://s2.ax1x.com/2020/01/31/1392KH.png "gmail配置02")

另外，若遇到提示
> 不允许访问账户

登录谷歌邮箱后，去[gmail的这个界面](https://accounts.google.com/b/0/DisplayUnlockCaptcha)点击允许。这种情况较为少见。

#### 设置QQ邮箱
***
在`设置>账户>POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务`下，开启`POP3/SMTP服务`

![qq邮箱配置01](https://s2.ax1x.com/2020/01/31/13cIKA.png "qq邮箱配置01")

此时坑爹的QQ邮箱会要求你用手机发送一条短信给腾讯，发送完了点一下`我已发送`

![qq邮箱配置02](https://s2.ax1x.com/2020/01/31/13c4vd.png "qq邮箱配置02")

然后你就能看到你的邮箱授权码了，使用邮箱账户加授权码即可登录，记下授权码

![qq邮箱配置03](https://s2.ax1x.com/2020/01/31/13cTbt.png "qq邮箱配置03")

![qq邮箱配置04](https://s2.ax1x.com/2020/01/31/13coDI.png "qq邮箱配置04")

#### 设置163邮箱
***
在`设置>POP3/SMTP/IMAP`下，开启`POP3/SMTP服务`和`IMAP/SMTP服务`并保存

![163邮箱配置01](https://s2.ax1x.com/2020/01/31/13WKZn.png "163邮箱配置01")

![163邮箱配置02](https://s2.ax1x.com/2020/01/31/13WQI0.png "163邮箱配置02")

现在点击侧边栏的`客户端授权密码`，并获取授权码，你看到画面可能和我不一样，因为我已经获取了授权码，所以只有`重置授权码`按钮，这里自己根据网站提示申请获取授权码，网易和腾讯一样恶心，需要你用手机给它发一条短信才能拿到授权码

![163邮箱配置03](https://s2.ax1x.com/2020/01/31/13WMaq.png "163邮箱配置03")

#### Telegram bot
***
上面介绍了三种邮箱的设置方法，如果你不想使用邮件推送，也可以使用 Telegram bot，灵活配置。在`config.php`文件中，将`telegram`下的
`enable`的值改为`true`，即可启用 Telegram bot，同样的，将`mail`下的`enable`的值改为`false`即可关闭邮件推送方式。
Telegram bot 对应两个配置值，一个是`chatID`，通过使用你的 Telegram 账户发送`/start`给`@userinfobot`即可以获取自己的id，另一个是
`token`，你的 Telegram bot 令牌，你会创建 Telegram bot 就知道怎么获取，不多赘述。
如何创建一个 Telegram bot 请参考官方文档：[点我了解](https://core.telegram.org/bots#6-botfather)

*与通知相关的设置到此就完成了，下面可以愉快的配置本程序了* :)

### 🚧 配置脚本
所有操作均在Centos7系统下进行，其它Linux发行版大同小异
#### 获取源码
```bash
$ mkdir -p /data/wwwroot/freenom
$ cd /data/wwwroot/freenom

# clone本仓库源码
$ git clone https://github.com/luolongfei/freenom.git ./
```

#### 配置过程
```bash
# 复制配置文件模板
$ cp .env.example .env

# 编辑配置文件
$ vim .env

# .env文件里每个项目都有详细的说明，这里不再赘述，简言之，你需要把里面所有项都改成你自己的。需要注意的是多账户配置的格式：
# e.g. MULTIPLE_ACCOUNTS='<账户1>@<密码1>|<账户2>@<密码2>|<账户3>@<密码3>'
# 当然，若你只有单个账户，只配置FREENOM_USERNAME和FREENOM_PASSWORD就够了，单账户和多账户的配置会被合并在一起读取并去重。

# 编辑完成后，按“Esc”回到命令模式，输入“:wq”回车即保存并退出，不会用vim编辑器的问下谷歌大爷:)
```

### 🎈 添加计划任务
#### 安装crontabs以及cronie
```bash
$ yum -y install cronie crontabs

# 验证crond是否安装及启动
$ yum list cronie && systemctl status crond

# 验证crontab是否安装
$ yum list crontabs $$ which crontab && crontab -l
```

#### 打开任务表单，并编辑
```bash
$ crontab -e

# 任务内容如下
# 此任务的含义是在每天早上9点执行/data/wwwroot/freenom/路径下的run文件
# 注意：某些情况下，crontab可能找不到你的php路径，下面的命令执行后会在freenom_crontab.log文件输出错误信息，你应该指定php路径：把下面的php替换为/usr/local/php/bin/php（根据实际情况）
00 09 * * * cd /data/wwwroot/freenom/ && php run > freenom_crontab.log 2>&1
```

#### 重启crond守护进程（每次编辑任务表单后都需此步，以使任务生效）
```bash
$ systemctl restart crond
```
若要检查`计划任务`是否正常，你可以将上面的任务执行时间设置在几分钟后，然后等到任务执行完成，
检查`/data/wwwroot/freenom/`目录下的`freenom_crontab.log`文件内容，是否有报错信息。常见的错误信息如下：
- /bin/sh: php: command not found
- /bin/sh: /usr/local/php: Is a directory

> 解决方案：
>
> 先执行
> ```bash
> # 确定php的位置，一般输出为“php: /usr/local/php /usr/local/php/bin/php”，选长的那个即：/usr/local/php/bin/php
> $ whereis php
> ```
> 现在我们知道php的路径是`/usr/local/php/bin/php`（根据你自己系统的实际情况，可能不同），然后修改表单任务里的命令，把
> 
> `00 09 * * * cd /data/wwwroot/freenom/ && php run > freenom_crontab.log 2>&1`
> 
> 改为
> 
> `00 09 * * * cd /data/wwwroot/freenom/ && /usr/local/php/bin/php run > freenom_crontab.log 2>&1`
> 
> 更多参考：[点这里](https://stackoverflow.com/questions/7397469/why-is-crontab-not-executing-my-php-script)
>
当然，如果你的`计划任务`能正确找到`php路径`，没有错误，那你什么也不用做。

*至此，所有的配置都已经完成，下面我们验证一下整个流程是否走通*:)

### ☕ 验证
你可以先将 `.env` 中的 `NOTICE_FREQ` 的值改为1（即每次执行都推送通知），然后执行
```bash
$ cd /data/wwwroot/freenom/ && php run
```
不出意外的话，你将收到一封关于域名情况的邮件。

<hr>

### 🤣 本项目最简单的使用方法
上面说了一堆都是基于你有自己的 `VPS` 的情况下，如果没有 `VPS` 又想自动续期 `Freenom` 的域名，或者单纯不想配置那么多东西，
可以直接在 `Github Actions` 上跑本项目，`Github Actions` 会为项目创建一个虚拟环境，并在执行后自动销毁。

#### 只需简单 3 步

1、Fork 本仓库

2、在你 Fork 的本仓库下的 `Settings` -> `Secrets` 页面追加以下几个 `secret` 变量

FREENOM_USERNAME、FREENOM_PASSWORD、MULTIPLE_ACCOUNTS、MAIL_USERNAME、MAIL_PASSWORD、TO、MAIL_ENABLE、TELEGRAM_CHAT_ID、
TELEGRAM_BOT_TOKEN、TELEGRAM_BOT_ENABLE、NOTICE_FREQ

（注：此处列出了所有可用的变量名，每个变量具体的含义以及格式请参考本项目的 `.env.example` 文件内的注释。另一个需要注意的是，
`MAIL_USERNAME`、`MAIL_PASSWORD` （即发信机器人邮箱）尽可能使用 `163邮箱` 或者 `QQ邮箱`，而非之前推荐的 `Gmail`。
因为谷歌的安全机制，每次在新设备登录 `Gmail` 都会先被限制，需要手动解除限制才行，
而 `Github Actions` 每次创建的虚拟环境都会分配一个新的设备 `IP`，相当于每次都是从新设备登录 `Gmail`，
而我们不可能每次都去手动为 `Gmail` 解除登录限制，所以这种机制会导致无法发出通知邮件）

3、在心里默念作者好帅，给个 `star` 并把本项目推荐给更多的人（用的人越多，作者更新的动力越足），把省下的买 `VPS` 的巨款 [打赏一点点儿](#-捐赠-donate) 给作者 😝

<hr>

遇到任何问题或 Bug 欢迎提 [issues](https://github.com/luolongfei/freenom/issues) （请按模板格式提 `issues`，以便作者更快复现你的问题），
如果 `Freenom` 改变算法导致此项目失效，请提 [issues](https://github.com/luolongfei/freenom/issues) 告知，我会及时修复，本项目长期维护。
欢迎 `star` ~

### 🍺 信仰

![南京市民李先生](https://s2.ax1x.com/2020/02/04/1Bm3Ps.jpg "南京市民李先生")
> 
> 认真是我们参与这个社会的方式，认真是我们改变这个社会的方式。  ——李志

### ❤ 捐赠 Donate
如果你觉得本项目真的有帮助到你并且想回馈作者，感谢你的捐赠。
#### PayPal: [https://www.paypal.me/mybsdc](https://www.paypal.me/mybsdc)
> Every time you spend money, you're casting a vote for the kind of world you want. -- Anna Lappe

![pay](https://s2.ax1x.com/2020/01/31/1394at.png "Donate")

![每一次你花的钱都是在为你想要的世界投票。](https://s2.ax1x.com/2020/01/31/13P8cF.jpg)

**你的star或者小额打赏是我长期维护此项目的动力所在，由衷感谢每一位支持者，“每一次你花的钱都是在为你想要的世界投票”。**

### 📋 捐赠名单 Donate List
非常感谢「 [这些用户](https://github.com/luolongfei/freenom/wiki/Donate-List) 」对本项目的捐赠支持！

### 🌚 作者
- 主程序以及框架：[@luolongfei](https://github.com/luolongfei)
- 英文版文档：[@肖阿姨](#)

### 🎉 鸣谢
- [PHPMailer](https://github.com/PHPMailer/PHPMailer/) （邮件发送功能依赖此库）
- [guzzle](https://github.com/guzzle/guzzle) （Curl库）

### 🥝 开源协议
[MIT](https://opensource.org/licenses/mit-license.php)
