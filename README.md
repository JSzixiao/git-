# git常用命令

* 初始化一个空仓库
	* git init
* 配置用户名和邮箱
	* git config --global user.name "jason"
	* git config --global user.email "jason@jason.com"
* 添加文件
	* git add test.test
* 提交文件
	* git commit -m "add test.test"
* 从远程仓库克隆
	* git clone https://github.com/Gazler/cloneme
* 从远程仓库克隆到某个目录
	* git clone https://github.com/Gazler/cloneme test_dir
* 增加忽略名单
	* vim .gitignore
	* 增加 .DS_Store
* 忽略某类文件，但是排除其中特定的一个
	* vim .gitignore
	* 增加 *.a
	* 忽略 !lib.a
* 查看状态
	* git status
		* untracked - 新增的文件，Git 根本不知道它的存在
		* not staged - 被索引过又被修改了的文件
		* staged - 通过 git add 后被即将被提交的文件
* 删除一个文件
	* git rm test.test
* 有时候执行 add 操作的时候不小心把多余的文件 add 进去了，这时我们需要把它从 staging area 移除出来，但不能删除文件
	* git rm test.test --cached
* 把当前未提交的改动「复制」到另一个地方暂存起来，恢复的时候执行 git stash pop 即可
	* git stash
* 重命名文件
	* git mv aaa.aaa bbb.bbb
* 移动文件
	* git mv *.aaa aaa/
* 查看git提交历史
	* git log
* 打tag
	* git tag newtag
* 提交tag列表
	* git push --tags
* 提交之后发现漏掉了某些文件 amend
	* git add forgotten_file.rb
	* git commit --amend
	* 命令执行后会调用默认的编辑器编辑 commit message，它会自动带出之前的 message，如果不需要修改，直接保存退出即可
* 覆盖提交日期，默认提交的时候记录的是当前系统时间
	* git commit -m "test" --date "2016-05-08 23:59:59"
* 本来只想提交一个文件，结果意外地把另一个文件也 add 了，怎么把它搞出来？
	* git reset 可以用来改变 HEAD 的位置或把文件从 staging area 移除出来，但并不会丢失任何的修改
	* git reset to_commit_second.rb
* 提交太快，多提交了一个文件怎么办？
	* git reset 可以帮我们把当前的 HEAD 重置到指定的位置，这里是倒数第二个，所以是：HEAD~1。但我们希望变更还保持在 staging area，不然还得 add 一次挺麻烦，那可以加上 --soft 参数。
	* git reset HEAD~1 --soft
*  **`rm` 是删除一个文件，`reset` 是从 `commit` 中移出或 `staging area` 中移出**
* 将某个文件的变更恢复到之前的状态
	* git checkout config.rb 
* 为了与别人合作，我们需要把 Git 仓库 push 到远端仓库上去，远端仓库对应就是 remote
	* git remote
* 远端仓库除了有名称，还得有一个 URL 地址
	* git remote -v
* 拉取代码
	* git pull origin master
* 添加远程主机
	* git remote add origin https://github.com/githug/githug
* 把一个分支的修改合并到当前分支
	* git rebase
* 查看文件变化情况
	* git diff app.rb 
* 执行git blame;命令时，会逐行显示文件，并在每一行的行首显示commit号,提交者,最早的提交日期
	* git blame
* 创建分支但是不切换分支
	* git branch test_code
* 创建并切换分支
	* git checkout -b my_branch
* 切换到tag
	* git checkout v1.2
* 当存在同名的 tag 和分支时，git 不知道我们究竟是要 checkout 到 tag 还是到分支，它认为分支的优先级更高。这时就要显式地告诉 git 我们是要切换到 tag。
	* git checkout tags/v1.2
* 有时忘记开新的分支，就修改并提交了代码。开分支的时候默认是基于最新的一次提交的，但我们也可以指定参数使其基于任一次提交。
	* git branch test_branch HEAD~1
* 删除分支
	* git branch -d delete_me
* 有时候在特性分支上提交了代码，但还不能并入主干，却又希望和别的同事分享（比如需要他们帮做 Code Review），那就需要把分支 push 到远程仓库中去。
	* git push origin test_branch
* 将另一个分支并入当前工作分支
	* git merge feature //将feature分支merge到当前分支
* 当远程仓库有更新，但我们并不想合并到本地仓库，只想把代码拿下来看看，我们会用到 fetch 命令
	* git fetch origin
* We are using a git rebase workflow and the feature branch is ready to go into master. Let's rebase the feature branch onto our master branch.
	* git rebase master feature
* 将版本库未打包的松散对象打包，优化对象的存储
	* git repack -d
* 将某个commit信息从分支上选出来，合并到另一个分支上
	* 查看分支上某个文件的log
	* git log new-feature -p README.md
	* 将某个commit信息合并到当前分支
	* git cherry-pick ca32a6dac7b6f97975edbe19a4296c2ee7682f68
* 文件内容搜索定位工具
	* 查看TODO相关
	* git grep "TODO"
* 简化log信息
	* git log --oneline
* 修改后commit的commit
	* git rebase -i HEAD~2
	* r，表示：使用 commit，并且修改 commit message。
	* s，意思是使用这个 commit，但将它合并到前一个 commit 中去。
* 在 merge 特性分支时，把所有的新提交合并成一个
	* git merge long-branch --squash
	* git commit -m "merge long-branch"
* 当提交顺序错乱时，使用 `git rebase -i` 进行调整
	* git rebase -i HEAD~2
* 跑自动化测试，找寻commit中提交的bug
	* git bisect run make test