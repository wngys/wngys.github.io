# Git分支管理与冲突解决

常见的工作流程:

1.Git branch -b b1 本地新建b1分支并切换

2.本地编辑代码 git status add commit

3.Git push origin b1 建立远程b1分支并将本地b1分支推送到远程b1分支

4.Git checkout master 本地切换回master分支

5.Git pull origin master 从远程仓库更新本地master分支 保持好习惯 此时远程仓库master可能被别人push更新 

6.Git merge b1  本地两个分支merge合并，可能有冲突（本地两个分支冲突，本地master分支是刚刚从远程仓库pull的)

理论上b1分支包含未pull前master分支的状态，因为本地要更改先变b1分支

7.git status | git push origin master 解决完冲突后， 本地master分支拥有最新内容，将其push到远程master分支

*wngys:* 

b1

master

-------------------------------------------------------------------------------------------------------------

*GitHub:*

remote/b1

remote/master

remote/b2

-------------------------------------------------------------------------------

*lzh:*

b2

master

远程分支与本地分支一一对应，远程master分支不要轻易push，两个人共同维护