# 2022-09
> ## 生成镜像
```shell
~$ sudo docker build -t fakeovm:v1 .
```
> ## 保存镜像
```shell
~$ docker save -o fakeovm.tar 7256b6240eb9 #镜像Id
```
> ## 删除镜像文件
```powershell
> rm .\fakeovm.tar
```
> ## sftp 连接服务器
```powershell
> sftp yx@10.8.8.8
```
> ## 上传镜像
```powershell
sftp>  put fakeovm.tar
```
> ## 删除服务端镜像
```shell
~$ rm fakeovm.tar
```
> ## 服务端加载镜像
```shell
~$ sudo docker load -i fakeovm.tar
```
