# XMPP聊天服务器OpenFire的部署

## 关于XMPP

...

## OpenFire的安装

1. 直接前往OpenFire网站，下载`deb`安装包。

2. 先安装并配置Postgresql数据库，参考[PostgreSQL新手入门](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)。

3. 安装第一步下载的OpenFire安装包。完成后在`/etc/init.d/`中就已经有了`openfire`的控制脚本，可通过`sudo service openfire start/stop/restart`进行其服务的启动/停止/重启了。同时OpenFire的安装目录为`/usr/share/openfire`

4. 启动`openfire`服务，通过浏览器加载`http://localhost:9090/`，填入相关选项、数据库连接参数及管理员邮箱密码等。

5. 重新进入`http://localhost:9090/`, 此时要求输入用户名`admin`，及上一步设定的管理员密码，进入系统后进行其它设置。

## OpenFire文件传输的支持

默认安装的OpenFire并不支持文件传输。为实现文件传输，需要加入一些系统属性。在“服务器” -> “系统熟悉” -> “添加新属性”中，加入以下属性（参考[文件传输](https://github.com/KangLin/RabbitIm/blob/master/docs/Books/FileTransfer.md)）：

```
xmpp.filetransfer.enabled = true
xmpp.proxy.service=proxy
xmpp.proxy.enabled=ture
xmpp.proxy.port=7777
xmpp.proxy.externalip=127.0.0.1
```

就可以实现XMPP客户端之间的文件传输了，但此种文件传输尚不能支持离线托管的文件传输，离线托管文件传输仍需进一步配置。

## OpenFire用户自主注册

默认用户（及用户注册）是通过管理员手动完成的，而为实现自主注册，需要安装一个名为`Registration`的插件，该插件只有英文界面，但具有`i18n`特性，故可通过加入相应的`zh_CN` properties文件，对其进行界面汉化，步骤如下。

1. 将安装目录（`/usr/share/openfire`）下`plugins`中的`registration.jar`文件拷贝到用户目录（需要使用`sudo`），并将其`chown`为一般用户所有。

2. 将`plugins`下的`registration/i18n/`中的原版语言`properties`文件拷贝到用户目录，并`chown`为一般用户所有。

3. 使用任何的文本编辑器，对原版语言`properties`文件进行翻译编辑，完成后加以保存。

4. 运行`native2ascii -encoding UTF-8 registration_i18n.properties registration_i18n_zh_CN.properties`，得到适用于Java的语言文件。

5. 使用任意的解压软件，打开`registration.jar`这个jar压缩包，将得到的语言文件，拖入到打开的压缩包的`i18n`文件夹中，关闭解压软件即可。

6. 使用`sudo`将该`jar`文件，拷贝至OpenFire安装目录的`plugins`文件夹中（`/usr/share/openfire/plugins`）。

7. 在OpenFire的web管理控制台的“插件”->“插件”下，对`Registration`插件进行重启，后即可得到中文化的自主注册页面了，汉化完成。

## 在OpenFire web登陆页加入注册链接


