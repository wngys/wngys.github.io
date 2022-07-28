本地仓库远程连接Github已知三种方法:

# 1. token

相关文章：

[【Github】remote: Support for password authentication was removed on August 13, 2021.](https://blog.csdn.net/Joy_Cheung666/article/details/119832970)

一、在github上生成token

```
<令牌>
```

二、输入git命令

```
git remote set-url origin  https://<令牌>@github.com/<USERNAME>/<REPO>.git
```

# 2. http

```
git remote add origin https://github.com/wngys/<仓库名>.git
```

不要使用：

```
git remote add origin git@github.com:wngys/<仓库名>.git
```

此方法为SSH连接方法，经过一段时间没有成功解决





