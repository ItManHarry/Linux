# 防火墙

## 查看防火墙命令

```
	systemctl status firewalld
```

## 关闭防火墙 - 临时性关闭

```
	systemctl stop firewalld
```

## 启用防火墙 - 临时性开启

```
	systemctl start firewalld
```

## 关闭防火墙 - 永久关闭

```
	systemctl disable firewalld
```