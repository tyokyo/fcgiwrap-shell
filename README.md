
# 网页执行shell脚本，python其他脚本也是可以的只用替换相关命令即可

## 图例
![](https://img.jinchuang.org/github/fcgiwrap-shelllog1.png)
![](https://img.jinchuang.org/github/fcgiwrap-shelllog2.png)
![](https://img.jinchuang.org/github/fcgiwraphome.png)
![](https://img.jinchuang.org/github/fcgiwrapllist1.png)
![](https://img.jinchuang.org/github/fcgiwrapdisk.png)
![](https://img.jinchuang.org/github/fcgiwrapps.png)
![](https://img.jinchuang.org/github/fcgiwrap-server.png)
![](https://img.jinchuang.org/github/fcgiwrapssh.png)
![](https://img.jinchuang.org/github/fcgiwrapuptime.png)
![](https://img.jinchuang.org/github/fcgiwrap-shellmenu.png)


## 说明

- :gem: 基本功能就是把linux命令可以放到网页中去完成
- :gear: 定时刷新数据，时间自定义
- :globe_with_meridians: 本地和远程机器都可以，远程机器数据返回会慢一点
- :triangular_ruler: 页面样式比较low，你可以改为你想要的样式
- :rocket: 参考模板可以添加修改各种你想执行的命令
- :1234: 数据显示样式可以自定义修改

## 安装使用
安装文档: [Nginx支持web界面执行bash.python等脚本](https://me.jinchuang.org/archives/114.html)
如果要在远程机器上执行命令，需要安装sshpass 命令，或者使用export 或者使用秘钥认证都行

NGINX 配置：
```bash
location ~ ^/jcmon/api/ {  #这里的后缀匹配根据需要修改
	gzip off;
	fastcgi_pass  unix:/tmp/fcgiwrap.socket;
	#fastcgi_index index.cgi;
	include fastcgi_params;
	fastcgi_param  SCRIPT_NAME        $document_root$fastcgi_script_name;
}

```
具体执行的脚本例子【基本就是html+shell的结合，html代码需要用echo " "方式】：
```bash
#!/bin/bash
echo "Content-Type:text/html;charset=utf-8"
echo "" 
#echo "<script>window.setInterval(function(){
#	window.location.reload();
#},1000);</script>"
echo "<meta http-equiv="refresh" content="60">"
echo '<div style="padding-left:10px;">'
ip="这里改为你的机器ip即可"
echo '<h1 style="color:red;border-left:4px solid;padding:4px;">硬盘使用情况</h1>'
echo '<h5 style="color:#848484;">'
dd=`date`
echo "统计时间: $dd [60s刷新一次] 【当前机器ip: $ip】"
echo '</h5>'
echo '<pre style="border-left: 4px solid rgb(12, 40, 245);padding:5px;color:#fff;">'
#本地服务器
df -hT

#远程服务器
#sshpass -p 'passwd' ssh root@$ip -o StrictHostKeyChecking=no 'df -hT'
echo '</pre>'
```

