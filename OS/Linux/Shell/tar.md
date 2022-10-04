# tar 

* compress .gz
命令格式：tar -zcvf 压缩文件名.tar.gz 被压缩文件名
```shell {.line-numbers}
tar -zcvf file.tar.gz file.tar
```
* 解压到指定目录 
```shell {.line-numbers}
tar -xvf dotnet-sdk-6.0.400-linux-x64.tar.gz -C ./dotnet-sdk-6.0
```