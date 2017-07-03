Zabbix BearyChat AlertScript
========================

关于
----

这是一个简单的脚本：
1. 关联 [Zabbix](http://www.zabbix.com/)
2. 当 `Zabbix` 事件触发时，发送消息至 [BearyChat Zabbix Robot](https://bearychat.com/integrations/zabbix)

#### 适用版本
Zabbix 1.8.x 以上(包含 2.2， 2.4和3.x！)

安装
------------

### 脚本

[`bearychat.sh` 脚本](https://github.com/bearyinnovative/zabbix-bearychat-alertscript/raw/master/bearychat.sh) 需要放置在 `Zabbix servers` 配置文件 (`zabbix_server.conf`) 的 `AlertScriptsPath` 显示的目录下， 并且脚本可以被运行 `zabbix_server` 的用户运行(**Executable**):

	[root@zabbix ~]# grep AlertScriptsPath /etc/zabbix/zabbix_server.conf
	### Option: AlertScriptsPath
	AlertScriptsPath=/usr/local/share/zabbix/alertscripts

配置好后效果如下

	[root@zabbix ~]# ls -lh /usr/local/share/zabbix/alertscripts/bearychat.sh
	-rwxr-xr-x 1 root root 1.4K Dec 27 13:48 /usr/local/share/zabbix/alertscripts/bearychat.sh

如果你修改了 `zabbix_server.conf` 下的 `AlertScriptsPath` 字段(或者其他字段)， `Zabbix server` 都需要重新启动。

配置
-------------

### BearyChat web-hook

你需要在你所在的 BearyChat 团队中新建一个 `Zabbix 机器人`([https://your-team-subdomain.bearychat.com/dashboard/robots](https://your-team-subdomain.bearychat.com/dashboard/robots))

![](https://raw.githubusercontent.com/bearyinnovative/zabbix-bearychat-alertscript/master/imgs/hook.png)

只需要复制上面截图中的 **Hook 地址**

    https://hook.bearychat.com/XXX/incoming/XXXXXXXXXXXXXXXXXXXXXXX

确认你的 `Zabbix 机器人` 配置没有问题后，修改 `bearychat.sh` 脚本：

	# BearyChat zabbix web-hook URL
	url='https://hook.bearychat.com/XXX/zabbix/XXXXXXXXXXXXXXXXXXXXXXX'

### Zabbix配置

登陆`Zabbix Web UI`后（请确保你有**管理员super-administrator**的权限），选择`Administration` -> `Media Types` -> `Create media type`, 创建一个媒体类型（`media typs`）：

* **Name**: BearyChat
* **Type**: Script
* **Script name**: bearychat.sh

![](https://raw.githubusercontent.com/bearyinnovative/zabbix-bearychat-alertscript/master/imgs/media.png)

确认 `enabled` 选项被选中后，点击 “Save” 保存这个媒体类型。

然后，选择 `Administration` -> `Users` -> `Create User` 添加一个用户，在 `Media` 分类中添加一个 `BearyChat` 类型的 `Media`：

![](https://raw.githubusercontent.com/bearyinnovative/zabbix-bearychat-alertscript/master/imgs/add-user.png)

### 测试

在配置好脚本（`bearychat.sh`）后，可以在终端运行命令：

    $ bash bearychat.sh 'some-channel' PROBLEM 'Oh no! Something is wrong!'

然后在 `BearyChat` 中就能看到一条推送：

![](https://raw.githubusercontent.com/bearyinnovative/zabbix-bearychat-alertscript/master/imgs/test.png)


更多信息
----------------
* [BearyChat Zabbix](https://bearychat.com/integrations/zabbix)
* [Zabbix-Slack-AlertScript](https://github.com/ericoc/zabbix-slack-alertscript)
* [Zabbix 2.2 custom alertscripts documentation](https://www.zabbix.com/documentation/2.2/manual/config/notifications/media/script)
* [Zabbix 2.4 custom alertscripts documentation](https://www.zabbix.com/documentation/2.4/manual/config/notifications/media/script)
* [Zabbix 3.x custom alertscripts documentation](https://www.zabbix.com/documentation/3.0/manual/config/notifications/media/script)
