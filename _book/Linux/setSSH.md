---
title: 配置SSH连接github
---
配置本机的ssh key
通过ssh keys就可以将本地的项目与Github关联起来

## 1. 检查本机ssh key
cd ~/.ssh
提示：没使用过Git就会显示：No such file or directory

## 2. 生成新的ssh keys
```bash
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
注意：-C为大写的C
接下来会让你输入密码
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
注意：输入密码时是不会显示密码的，依次输入就好了
如果显示为下界面，就设置ssh key成功了
## 3. 添加ssh key到Github
1. 搜索本机上的id_rsa.pub文件。或在C:\Documents and Settings\Administrator.ssh\id_rsa.pub路径下找到该文件。以记事本打开，复制其中的内容
2. 进入自己的Github，右上角齿轮setting---左边列表SSH keys---Add SSH key。将内容复制到文本框（不会取title名字）。
注意：这时Github会给你的邮箱发送一封邮件，打开邮件确认下就好了。

## 4. 测试
ssh -T git@github.com
如果是以下反馈

The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
输入yes

Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
这时候说明能够通过SSH链接到你的Github了，接下来完善一下你的个人信息。
Git会根据用户的名字和邮箱来记录提交。GitHub也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

## 5. 配置用户
```bash
git config --global user.name "Tim"#用户名
git config --global user.email  "tim@gmail.com"#填写自己的邮箱
```
