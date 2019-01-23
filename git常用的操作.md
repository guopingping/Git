1、获取远程分支代码
```
git clone https://xxxx.git
```
2、切换到develop分支
```
git checkout develop
```
3、从当前分支创建本地任务分支
```
git checkout -b branchName
```
4、查看修改
```
git status
```
5、将修改添加到暂存区
```
git add .   	  // 添加所有修改到暂存区
git add 指定文件  	//添加指定文件到暂存区
```
6、提交本地代码到本地仓库
```
git commit -m "备注"
```
7、推送本地提交到远程仓库
```
git push origin branchName
```
8、创建合并请求

**
## Rebase变基
**
提交代码前，请做如下操作，目的是优化分支树，使提交更简洁，并提前解决冲突
先确认以下两点：
1）. 在当前工作分支上是否执行了多次commit操作？（导致分支树杂乱）
2）. 原分支（develop）分支是否有新的代码提交，而当前工作分支没有这些最新代码？（合并可能产生冲突）
解决第一个问题，按如下操作执行：
合并多次commit，成为一次提交；

如图所示，合并BCD为B'
示例：
有三次提交

```
git commit -m "B"

git commit -m "C"

git commit -m "D"
```
查看日志
```
git log
```
合并
```
git rebase -i HEAD~3 // 建议通过这种方式，即合并最近的3次提交
或者 
git rebase -i --autosquash develop  // 相对develop分支变基
```
编辑提交的命令，第一条的pick不变，第二条第三条的pick改为s；
:wq保存之后，进入提交信息页面；
把不需要提交的备注信息用#注释掉，留下一个即可；
:wq保存之后，rebase完成；
通过git log查看日志；
则到此：三次提交合并为一次提交；
推送到远程分支：
```
git push origin branchName
```
远程分支已存在，强制推送：
```
git push origin branchName -f
```
**第二个问题：**
1. 把当前分支的基点前进，使其基点前进到原分支head, 即把原分支最新的代码同步到当前工作分支
```
		git fetch //同步远程仓库信息
		
		git checkout develop //切换到原分支
		
		git pull // 获取最新代码
		
		git checkout workBranchName// 切换到工作分支
		
		git rebase origin/develop // 变基操作，即把原分支最新的代码同步到当前工作分支
		
```
2.  如果原分支的最新代码和当前工作分支没有冲突，那rebase就会完成，可以通过git log查看原分支的提交是否已经在当前分支当中

3. 推送到远程仓库

4. 如果原分最新代码和当前工作分支有冲突，rebase会停止，此时需要解决冲突，一般通过如下操作继续：
```
		git status // 查看冲突的文件
```
去冲突文件中解决冲突，然后
```
		git add /src/xxx.js //添加冲突文件到暂存区

		git rebase --continue //继续rebase
```
5. 执行第3步操作，提交分支代码

6. 如果不想rebase，可以执行回滚，如下命令

	回滚rebase操作：
```
		git rebase --abort
```



