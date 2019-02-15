# Linux服务相关命令(Centos7)

## Systemctl
```
systemctl start 服务名(xxx.service)
systemctl restart 服务名(xxx.service)
systemctl stop 服务名(xxx.service)
systemctl reload 服务名(xxx.service)
systemctl status 服务名(xxx.service)
```
查看服务的方法
```
/usr/lib/systemd/system
```
查看服务的命令
```
systemctl list-unit-files |grep firewalld
systemctl --type service
```
通过systemctl命令设置自启动
```
自启动 systemctl enable service_name
不自启动 systemctl disable service_name
```
