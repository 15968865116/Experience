# git配置 （ubuntu）

## 下载git

```bash
sudo apt-get install git
```

## 配置用户

```bash
git config --global user.name "xxx"
git config --global user.email "你的邮箱地址"
```

## 生成rsa验证 用于gitlab等的访问权限

```bash
ssh-keygen -C 'you email address@gmail.com' -t rsa
```

## 准备上传私钥

进入文件夹， 并打开文件

```bash
cd ~/.ssh
gedit id_rsa.pub
```

将此私钥复制并上传到gitlab上就行