# 命令: firewall-cmd
> ### 注释：管理Linux防火墙

``` shell
~/$ sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
success
~/$ sudo firewall-cmd --reload
success
```