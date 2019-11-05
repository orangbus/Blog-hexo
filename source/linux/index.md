title: linux
desc: SSh远程链接工具、推荐Uinux发行版、扶墙脚本、google上网
date: 2019-07-02 19:26:00
---
<span style="color:red;">更多Linux配置请参考：</span>https://github.com/orangbus/Tool

> ## 通过lrzsz与Linux传文件

安装

```
sudo yum install lrzsz
```

上传文件

```
rz [回车]
```

下载文件

```
sz fileName.txt
```

转自：<https://birdteam.net/4479>

> ## SSh远程链接工具

FinalShell（推荐）：<http://www.hostbuf.com/>

Terminus（推荐）：<https://github.com/Eugeny/terminus>

Git (必备)：<https://git-scm.com/>

babun：<http://babun.github.io/>

> ## 推荐Uinux发行版

Manjaro：<https://manjaro.org/>

Deepin：<https://www.deepin.org/>

Ubuntu：<https://www.ubuntu.com/index_kylin>

Centos：<https://www.centos.org/><!--more-->

> ## 扶墙脚本

233Blog:https://233blog.com/ <br>
要求：Ubuntu 16+ / Debian 8+ / CentOS 7+ 系统<br>
推荐使用 Debian 9 系统，脚本会自动启用 BBR 优化。<br>
备注：不推荐使用 Debian 8 系统，因为 Caddy 申请证书可能会出现一些莫名其妙的问题
```
bash <(curl -s -L https://git.io/v2ray.sh)
```

逗比的脚本
脚本说明: Shadowsocks 一键安装管理脚本<br>
系统支持: CentOS6+ / Debian6+ / Ubuntu14+

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ss-go.sh && chmod +x ss-go.sh && bash ss-go.sh
```
