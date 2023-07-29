# Mac Git上传代码到GitHub

利用`git --version`查看是否安装了git

```sh
user@userdeMacBook-Pro % git --version
git version 2.39.2 (Apple Git-143)
```



## 配置git

```sh
git config --global user.name "名字"
git config --global user.email "example@.com"
```

可以通过利用ssh key的方式来实现免登录

1. **生成SSH密钥对（如果尚未生成）：** 如果您尚未生成SSH密钥对，请按以下步骤生成：

   打开终端（Linux或macOS）或Git Bash（Windows）并输入以下命令：

   ```sh
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

   在这里，将"[your_email@example.com](mailto:your_email@example.com)"替换为您在GitHub上注册的电子邮件地址。然后，按照提示选择保存密钥对的位置和设置密码（可选）。

2. **获取公钥：** 在您生成SSH密钥对的位置，会有两个文件：id_ed25519（私钥）和id_ed25519.pub（公钥）。您需要复制id_ed25519.pub文件中的内容。

3. **登录GitHub账号：** 打开GitHub网站（https://github.com/）并登录您的GitHub账号。

4. **导航至"Settings"（设置）：** 在页面右上角的下拉菜单中，选择"Settings"。

5. **选择"SSH and GPG keys"（SSH和GPG密钥）：** 在左侧导航栏中，选择"SSH and GPG keys"。

6. **点击"New SSH key"（新建SSH密钥）：** 点击页面右上角的"New SSH key"按钮。

7. **添加SSH公钥：** 在"Title"字段中，可以给您的密钥一个描述性的名称（例如，"My Personal Laptop"）。然后，在"Key"字段中粘贴之前复制的id_ed25519.pub文件中的内容。

8. **保存SSH密钥：** 确认公钥信息无误后，点击"Add SSH key"按钮。

9. 配置SSH客户端： 在本地计算机的~/.ssh/config文件中添加以下配置，用于启用SSH代理和保持会话等设置：

```sh
Host *
  ForwardAgent yes
  ServerAliveInterval 120
```

10. 重启SSH代理（可选）： 如果SSH代理尚未运行，可以通过以下命令启动SSH代理：

```sh
eval "$(ssh-agent -s)"
ssh-add
```

输入私钥的密码以将其添加到代理中。



### 上传文件到GitHub指定仓库

切换到待上传文件的目录

```sh
user@userdeMacBook-Pro % cd Documents 
```

利用`git add 文件名`的方式添加待上传的文件，通过`git status`来查看git的状态

```sh
user@userdeMacBook-Pro Documents % git add 指针带来的弹性.md 
user@userdeMacBook-Pro Documents % git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   "\346\214\207\351\222\210\345\270\246\346\235\245\347\232\204\345\274\271\346\200\247.md"

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.UTSystemConfig/
	.localized
```

 利用`git commit -m "注释信息"`，将代码上传到本地仓库

```sh
user@userdeMacBook-Pro Documents Documents % git commit -m "pointer filexibility"
[main 787b185] pointer filexibility
 1 file changed, 220 insertions(+)
 create mode 100644 "\346\214\207\351\222\210\345\270\246\346\235\245\347\232\204\345\274\271\346\200\247.md"
```

通过`git remote add 仓库别名 仓库地址`添加远程仓库

```sh
user@userdeMacBook-Pro Documents % git remote add origin git@github.com:tangling123/tangling.github.io.git
```

利用`git remote`查看添加的仓库

```sh
user@userdeMacBook-Pro Documents % git remote     
origin
```

可以看到orgin就是刚刚添加的远程仓库

通过`git push origin main`	将代码推送到远程仓库

```sh
user@userdeMacBook-Pro Documents % git push origin main
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 8 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (8/8), 1.35 MiB | 924.00 KiB/s, done.
Total 8 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To github.com:123/123.github.io.git
   d892bf3..787b185  main -> main
```

