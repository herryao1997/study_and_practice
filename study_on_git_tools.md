# GIT 
分布式，版本控制（回滚），软件  
git + github 线上线下版本控制

**安装git，并查看其版本在ubuntu中**

```bash
$ sudo apt-get install git
$ git --version
```

##  版本控制的流程
- 进入控制的文件夹（进入文件夹）
- 初始化（提名）
- 管理
- 生成版本

1. 进入文件夹并初始化管理
```bash
$ git init
```
2. 查看文件夹状态
```bash
$ git status
```
3. 添加修改，提交至暂存区
```bash
$ git add ./file_name/*
```
4. 如果是第一次使用，需要添加个人信息和邮箱（只需要一次）
```bash
$ git config --global user.email "you@example.com"
$ git config --global user.name "your name"
```
5. 生成一个版本v1，提交至版本库
```bash
$ git commit -m "v1"
```
6. 查看历史记录
```bash
$ git log
```
7. 回滚（返回之前保存的版本）
```bash
$ git reset --hard version_index
```
8. 回滚后想查看进入回滚之后的版本，但发现 $ git log 无法查看
```bash
$ git reflog
$ git reset --hard version_index
```

## git的工作区域
- 工作区域  
已管理文件>自动检测>新修改文件（红色）  
将新修改内容直接抹除
```bash
$ git checkout -- ./file_name/*
```
提交至暂存区域
```bash
$ git add ./file_name/*
```
- 暂存区域（绿色）
从暂存区域提交至版本库
```bash
$ git commit -m "version_name"
```
从暂存区退回到工作区
```bash
$ git reset Head file_name
```
- 版本库
版本库中选取过去的版本（退回至工作区已控制文件）
```bash
$ git reset --hard version_index
```
版本库退回到暂存区
```bash
$ git reset --soft version_index
```
版本库退回至工作区为控制文件
```bash
$ git reset --mix version_index
```
- github

## 分支
- 版本与版本之间通过指针联系，进行版本升级时保留不同的内容，未修改内容通过指针保留  
- 分支可以同时开启多个分别开发，开发完毕后合并使用
- 对不同开发进行环境的隔离

**场景：新一版本开发结束以后，发现上一版本出现了bug，如果回滚到上一版本debug会导致新开发功能失效，解决方案——分支开发**  
- 线上代码出现bug的紧急修复问题
- 在开发过程中开启一个分支进行开发  
- 出现bug后另开一个分支进行修复，修复结束后合并再跳回至开发分支  
- 继续开发开发结束后在合并到主分支完成开发

- 分支的查看以及新建开发分支  
默认的主分支为master内部只保留完全稳定的版本  
开发功能留在dev分支中（写代码）  
```bash
$ git branch
$ git branch dev
```
- 切换到其他分支进行开发如dev（development）
```bash
$ git checkout dev 
```
进行代码修改  
进行版版本保存
```bash
$ git add .
$ git commit -m "v1"
```
注意：在此保存的内容切换到其他分支会失效（环境隔离目的达成）
- 此时发现主分支代码出现bug，对其修复
首先退回到主分支master并创建一个分支bug用于debug并进入此环境
```bash
$ git checkout master
$ git branch bug
$ git checkout bug
```
完成debug  
保存版本退回至master分支并合并分支完成debug操作
```bash
$ git add .
$ git commit -m "v_debug"
$ git checkout master
$ git merge bug
```
bug分支没有意义，将其删除,此时查看分支bug分支已被删除  
```bash
$ git branch -d bug 
$ git branch
```
此时可以进入开发分支
```bash
$ git branch dev 
```
继续开发至结束保存版本并合并
```bash
$ git status
$ git add .
$ git commit -m "v2"
$ git checkout master
$ git merge dev
$ git branch -d dev
$ git branch
```
此时开发结束并完成上线  
**注意：合并过程中可能会产生冲突，两个分支同时某一行进行修改，此时会报出提示，并将两个分支修改的冲突部分全部保存  
     此时需要人为处理冲突，修复之后在提交即可（此现象只发生两分支在同一行操作导致冲突的情况下，其他情况会自动覆盖）** 
     
继续提交
```bash
$ git add .
$ git commit -m "v_final"
```

## github使用
github相当于百度云盘  
github可以理解为一个代码托管仓库（云端）其与git其实无关  
gitlab大型公司使用，防止泄漏  
github个人或小型公司使用

- **github的使用方法**
1. 注册帐号  
2. 创建仓库 
写入仓库名称
给出项目的简介
选择public（免费）
创建说明文件，license
创建https 地址信息
3. 本地代码 **首次推送**  
将地址转换成自定义名称如practice  
推送当前保存的分支master版本以及dev分支版本  
第一次使用会要求输入用户名和密码
```bash
$ git remote add practice https://github.com/herryao1997/study_and_practice.git
$ git push -u practice master
$ git push -u practice dev
```
4. 更换电脑 **首次克隆** github的代码  
首先进入目标目录然后执行克隆命令+仓库地址
```bash
$ git clone https://github.com/herryao1997/study_and_practice.git
$ git branch
```
此时查看分支之出现一个master分支  
但其实全部分支都被克隆下来，只需要按照仓库内容切换一次分支，即可激活所有分支,并可对分支执行操作进行开发
```bash
$ git checkout dev
$ git log
$ git branch
```

## 不同区域的协同开发  
- **位置1**
从github中克隆下远程内容
开发之前首先要将主分支内容合并到开发环境分支内容对其进行更新  
然后在dev分支下进行开发
```bash
$ git clone practice
$ git checkout dev
$ git merge master
```
开发结束上传代码至github
```bash
$ git add .
$ git commit -m "update1"
$ git push -u practice dev
```
- **位置2**  
从github中克隆下远程内容(第一次)  
注意：**此时已有内容，因此不需要克隆只需要对其进行更新即可**  
此外依旧：  
开发之前首先要将主分支内容合并到开发环境分支内容对其进行更新  
然后在dev分支下进行开发  
```bash
$ git pull practice dev
$ git checkout dev
$ git merge master
```
开发结束上传代码至github
```bash
$ git add .
$ git commit -m "update2"
$ git push -u practice dev
```
- **全部开发结束以后**  
将dev分支合并到master并上线  
将dev和master合并使其也新到最新版本
将dev分支一并上线  
```bash
$ git add .
$ git commit -m "final"
$ git checkout master
$ git merge dev
$ git push practice master
$ git checkout dev
$ git merge master
$ git push practice dev 
```

## 忘记推送开发结束并已提交至版本库的代码  
- 去到另一地点，先按照记忆进行继续开发，保存更新并推送至github  
- 回到工作地点，更新github中在另一地点开发的程序，与之前本地未上传的更新进行合并  
- 此时有可能会产生冲突  
- 注意此时会提示冲突，请按照提示冲突进入相关文件并按照标记位置合理作出选择，解决相关问题  
- 解决完成冲突之后继续开发，开发结束后执行之前的所有步骤，并且上传代码至github

## 一点补充关于git pull 命令  
其实git pull命令是两个命令的叠加结果  
git pull是将远程仓库github中的代码直接拉到工作区
而其又可以分为两步：  
1. 将远程仓库github中的代码先拉到版本库
2. 再将其更新到本地电脑的程序文件之中
即：  
```bash
$ git pull practice dev
#等同于
$ git fetch practice dev
$ git merge practice/dev
```
以上情况不需要使用，一般用git pull命令即可！

## git rebase命令  
### 场景一  
当在某一个分支中加载了许多的版本可以用git rebase合并之前的版本（多次提交记录）  
即：多个提交记录整合成一个提交记录
```bash
$ git rebase -i index_1 index_2
#如果版本很多可以直接用HEAD命令加~n，表示从第一个版本向下合并n个版本,如向下合并2个记录：
$ git rebase -i HEAD~2
```
输入以上命令后会产生如下提示信息：


将右上角的两个版本前面的pick转换成s完成合并并用:wq保存


在此之后会跳出修改名称的命令按照需求重新修改即可


这里修改为vfinal_this_time  
最终可以看到版本已经被合并

注意此时不推荐将本地的版本合并至github仓库的版本，可能会导致一些错误

### 场景二  
- 问题描述  
平时开发中，再不同的分支中各自更新，最终合并  
```bash
$ git checkout master
$ git merge dev
$ git log
```
此时git log显示的版本会产生分支，但在此命令下并不清晰  
此时使用git log --graph 可以看到分支  
```bash
$ git log --graph
```

但是结果过于杂乱可用git --graph --pretty=format:"%h %s" 即：  
```bash
$ git --graph --pretty=format:"%h %s"
```
其中%h表示哈西值，s%表示记录，其结果最终如下：  

![image-10.png](attachment:image-10.png)

- 开始合并分支中的记录  
首先切回到dev分支  
执行git rebase master 执行并基  
切换回master分支  
再将dev覆盖回来  
```bash
$ git checkout dev
$ git rebase master
$ git checkout master
$ git merge dev
$ git log --graph --pretty=format:"%h %s"
```

次过程会在历史版本中产生分支  
但这种修改会导致修改过程不够清晰，需要取舍  
此方法未必回事用到只是在此做一补充
### 场景三
当在多个地点进行工作时，每次git pull命令都会自动产生一个分支  
如何取消：  
不使用git pull  
之前提及过git pull practice dev = git fetch practice dev+ git merge practice/dev  
在此将以上命令更改为：  
```bash
$ git checkout dev
$ git fetch practice dev
$ git rebase practice/dev
```
如此覆盖操作将不会产生分叉  
### 一些注意事项
在rebase操作之后可能会产生一些冲突  
如果冲突发生系统一定会提示，因此一定要注意提示信息  
按照提示进行解决冲突  
解决冲突之后系统应该会提示需要你执行一些命令比如 git add .  
此时其实rebase命令是被暂停了的，并未被执行结束，因此一定要继续执行完成  
按照提示执行玩命令以后要执行 git rebase --continue  
```bash
$ git rebase --continue
```
在此举例：  
```bash
$ git checkout def
$ vim a1
#对文件a1进行修改
$ git add .
$ git commit -m "v1"
$ git checkout master
$ vim a1
#对文件a1进行修改，注意此时在master分支中
#如此操作合并时会产生冲突
$ git add .
$ git commit -m "v2"
$ git checkout dev
$ git rebase master
#此时会提示错误，原因之前已经说过，提示结果如下图
#修改结束后查看git status可以看到相关提示
$ git status
#下下图给出相关提示，按照提示执行完成rebase命令
#首先要更新修改后的文件
$ git add .
$ git rebase --continue
$ git log
#此时可以看到结果执行完毕
```

## 总结
- 添加远程链接（别名）
```bash
$ git remote add practice 地址
```
- 推送代码
```bash
$ git push -u practice dev
```
- 第一次克隆代码
```bash
$ git clone + 地址
```
- 下拉代码
```bash
$ git pull practice dev
#等价于如下
$ git fetch practice dev
$ git merge practice/dev
```
- 保持代码整洁（变基）  
```bash
$ git rebase 

```
- 记录图形展示
```bash
$ git log --graph --pretty==format:"%h %s" 
```

## git免密登录
- URL
```bash
$ git remote add practice https:username:password@github.com/...
$ git push practice master
```
