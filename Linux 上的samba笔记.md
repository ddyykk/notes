# Linux 上的samba笔记

- 添加共享路径的时候要写绝对路径
- 要对苹果设备支持的更好, 在[global]下面添加如下参数

```
min protocol = SMB2
fruit:delete_empty_adfiles = yes
vfs objects = fruit streams_xattr
fruit:metadata = stream
fruit:model = MacSamba
fruit:aapl = yes
```