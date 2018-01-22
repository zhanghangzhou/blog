Ip Tables
---
- Configuration File: /etc/sysconfig/iptables

- 开放指定区间端口
``` bash
vi /etc/sysconfig/iptables
-A INPUT -p tcp -m state --state NEW -m tcp --dport 4445:65535 -j ACCEPT
```

# 服务管理
- 启动
``` bash
service iptables start
```

- 停止
``` bash
service iptables stop
```

- 开机自启动
``` bash
chkconfig iptables --level 345 on
```

- 开机不启动
``` bash
chkconfig iptables off
```
