# 搭建NFS
## server
sudo apt-get install nfs-kernel-server  
sudo vim /etc/exports # 配置需要共享的目录,及相关设置  
```
#格式: 共享目录 共享ip(options)
#说明:
#ip: 限制可访问的ip,*为所有
#options:
## rw 读写
## sync 写操作同步,阻塞等待结果
## no_subtree_check 不检查父目录权限
## anonuid=1000,anongid=1000 一般期望对同一个用户能读写它自己的文件,但因server/client的uid/gid不一致,造成mapping异常的问题,因此,NFS服务统一使用匿名帐号作为操作帐号,client的读写操作继承该匿名帐号的权限.这里设定匿名帐号及匿名组解决权限问题.
## uid&gid请查看/etc/passwd
/home 192.168.0.104(rw,sync,no_subtree_check,anonuid=1000,anongid=1000)
```
sudo /etc/init.d/nfs-kernel-server restart #配置完后,重启服务  
showmount -e #显示已共享目录  
```
#如无意外,显示
/home 192.168.0.104
```


## client
sudo apt-get install nfs-common # 安装nfs客户端  
showmount -e 192.168.0.105 # 检查server共享目录,输出与上面一致  

mkdir /tmp/home105  
sudo mount -t nfs 192.168.0.105:/home /tmp/home105 -o nolock,tcp # 挂载  
sudo umount /tmp/home105 # 卸载  
Todo # 开机自动挂载  



