# volumes

## 1. 数据卷

数据卷是一个可供一个活多个容器使用的特殊目录，它绕过了ufs(文件系统)，可以提供多种有用的特性  
- 数据卷可以在容器之间共享和重用
- 对数据卷的修改会马上生效
- 对数据卷的更新，不会影响镜像
- 卷会一直存在，直到没有容器使用

## 2. 数据卷容器  

如果有一些持续更新的数据需要在容器之间共享，最好创建数据卷容器
数据卷容器，其实就是一个正常的容器，专门用来提供数据卷供其他容器挂载的

首先创建一个命名的数据卷容器 dbdata：
```
sudo docker run -d -v /dbdata --name dbdata training/postgres
```
然后在其他容器中使用 --volumes-from参数来从多个容器挂载多个数据卷。
```
sudo docker run -d --volumes-from dbdata --name db1 training/postgres
sudo docker run -d --volumes-from dbdata --name db2 training/postgres
```

还可以使用多个 --volumes-from 参数所挂载数据卷的容器自己并不需要保持在运行状态
```
sudo docker run -d --name db3 --volumes-from db1 training/postgres
```

使用--volumes-from 参数所挂载数据卷的容器自己并不需要保持在运行状态。如果删除了挂载的容器，数据卷并不会被删除，如果要删除一个数据卷，必须在删除最后一个还挂载着它的容器时使用docker rm -v 命令来指定同时删除关联的容器。这可以让用户在容器之间升级和移动数据卷。
