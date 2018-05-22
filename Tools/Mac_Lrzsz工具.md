# Mac安装Lrzsz工具
大家知道Linux的环境下传输文件最方便的工具是scp，但大多数情况开发是没有scp的使用权限的。所以Windows用户习惯了Xshell或Securecrt的rz和sz命令。   
rz 可以很方便的从客户端传文件到服务器，sz也可以很方便的从服务器传文件到客户端，就算中间隔着跳板机也不影响。在mac下试了一下，mac的终端是不支 持的，需要下载item2。另外不能在mac下用expect 自动登录服务器，执行rz或sz 否则终端会挂掉。


* 1.先安装item2，item2 Mac命令行终端工具    
     官网下载地址:​​http://iterm2.com/downloads.html  
* 2.使用brew 安装lrzsz  
   $> sudo brew install lrzsz  
   安装完成后检查:  
   ls -alh /usr/local/bin/sz 是否存在

* 3.下载iterm2-zmodem
```bash
wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-send-zmodem.sh  
wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-recv-zmodem.sh  
chmod 755 iterm2-*
```
* 4.给终端Iterm2 添加触发trigger

    打开iterm2 ------  同时按 command和,键 ------- Profiles ----------  Default ------- Advanced ------ Triggers的Edit按钮

    ![img](https://github.com/LeoDominicXu/DevelopNote/blob/master/Tools/img/lrzsz_00.jpeg)  
    ![img](https://github.com/LeoDominicXu/DevelopNote/blob/master/Tools/img/lrzsz_01.jpeg)  
    ![img](https://github.com/LeoDominicXu/DevelopNote/blob/master/Tools/img/lrzsz_02.jpeg)
