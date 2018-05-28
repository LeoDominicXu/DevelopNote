# 使用Nginx建立一个文件服务器

## 安装nginx
本人是Mac系统，首先需要安装brew，如果没有brew需要先安装。
```bash
 ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

 brew install nginx -y
```

## 修改配置
配置文件在：/usr/local/etc/nginx/nginx.conf  
如果需要引入其他配置文件，可以在nginx.conf末尾添加：include /usr/local/etc/nginx/servers/*.conf .
```bash
  location / {
      #root   html;
      #index  index.html index.htm;
      root /data/files; #文件根目录
      autoindex on; ##显示索引
      autoindex_exact_size on; ##显示大小
      autoindex_localtime on;   ##显示时间
  }
```

## 启动Nginx
```bash
  sudo nginx #启动
  sudo nginx -s reload #重启
```

## 浏览器http://[nginx-ip]:8080访问既可
