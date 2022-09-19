# docker 命令

> ## 查看镜像
* 查看docker images帮助
``` console
~$ docker images --help
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs
```

* 查看所有镜像
``` bash
~$ sudo docker images
```
> ## 查看容器
* 查看docker ps帮助
``` console
~$ docker ps --help

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all
                        states) (default -1)
  -l, --latest          Show the latest created container (includes all
                        states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display container IDs
  -s, --size            Display total file sizes

```



* 查看运行的容器
``` bash
~$ sudo docker ps
```
* 查看所有容器
``` bash
~$ sudo docker ps -a
```

> ## 从镜像启动一个新的容器
* 直接启动
``` bash
~$ sudo docker run a3746378823  #镜像id
```
* 后台启动
``` bash
~$ sudo docker run -d a3746378823  #镜像id
# -d, --detach 
#    Run container in background and print container ID
```
* 后台启动并指定主机与容器的映射端口
``` bash
~$ sudo docker run -d -p 81:80 a3746378823  #镜像id
# -d, --detach
#    Run container in background and print container ID
# -p, --publish list 
#    Publish a container's port(s) to the host
# 81 is host port, 80 is container port.
```

> ## 启动一个已存在的容器
* 直接启动
~~~ bash
~$ sudo docker start be2345234  #容器id
~~~