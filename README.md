# 设置基于 Strongswan IPSec (& L2TP) 的 VPN 服务器

forked from [kitten/setup-strong-strongswan](https://github.com/kitten/setup-strong-strongswan)

原项目 [UNMAINTAINED]  已停止维护

部署一个使用 L2TP/IPSec 预共享密钥方式的 IPSec VPN（基于 StrongSwan 和 xl2tpd 实现） 

本部署脚本在原脚本的基础上添加了以下内容:

> 1. 添加项目依赖的安装
> 2. 更新 strongswan 版本，目前为 5.8.4
> 3. 添加 save-keys 插件
> 4. 添加 strongswan 日志，路径为 /var/log/strongswan

支持 IPSec 交互密钥 IKEv1、IKEv2、ESP 的导出，以便使用 Wireshark 进行解析

## 支持

**适用于 Ubuntu 和 Debian 系统**

* Ubuntu 18.04 (已测试)

* Ubuntu 19.10 (已测试)

如果不需要记录交互密钥，请自行删除部署脚本中以下内容：

1. strongswan 编译选项中的 --enable-save-keys
2. strongswan.conf 文件中的 save-keys 配置
3. /etc/strongswan/wireshark-keys 文件夹

如果不需要记录日志，请自行删除部署脚本中以下内容：

1. strongswan.conf 文件中的 filelog 配置


## 安装

本脚本部署时不需要域名和公共 IP

```
curl -L -O https://raw.githubusercontent.com/Salted-Fiish/setup-strong-strongswan/master/setup.sh
chmod +x setup.sh
sudo ./setup.sh
```

部署脚本将引导你完成安装过程。脚本运行过程中会要求输入以下 VPN 的凭据：

- username
- password
- PSK (pre-shared key)

如果要升级 strongswan 版本，只需再次运行脚本即可。记得提前备份自定义的 IPSec 配置文件。

## 使用

部署脚本在 /etc/init.d/ 文件中安装了 `vpn-assist` 脚本

Systemd 向后兼容这些脚本，因此你可以使用 `start|stop|restart` 来操作 VPN 服务器

这也实现了 VPN 服务的开机启动

你可以修改 `/etc/ipsec.secrets` 和 `etc/ppp/chap-secrets` 来对 VPN 服务器的登录账号进行管理

## 卸载

下载 Strongswan 源码并且执行:

```
make uninstall
```

之后手动卸载 `xl2tpd` 并删除 `/etc/init.d/vpn-assist`

## 客户端使用

**MacOS**

1. 新建VPN
2. 选择类型为 Cisco IPSec
3. 填写服务器地址、账户名称和密码
4. 点击认证设置，填写共享的密钥
5. 点击应用


