# Git版本控制

- Git三种状态
    - 已修改(modified)：表示修改了文件，但还没保存到数据库中
    - 已暂存(staged)：表示对一个已修改文件的当前版本做了标记，使其包含在下次提交的快照中
    - 已提交(committed)：表示数据已经保存在本地数据库中

- 查看Git的所有配置以及它们所在的文件
    - `git config --list --show-origin`

- 设置用户名和邮件
    - `git config --global user.name "xxxx"`
    - `git config --global user.email xxxxx`
    - 针对当前仓库可以使用`--local`

- 设置文本编辑器
    - `git config --global core.editor xxxxx`

- 检查某一项配置
    - `git config <key>`
    - 例如：`git config user.name`

- 获取帮助
    - `git help <verb>`
    - 例如：`git help config`
    - 简明帮助使用`-h`
    - 例如：`git add -h`


## 获取Git仓库

1. 在已存在的目录中初始化仓库
      1. cd 到目标目录下
      2. `git init`, 此时目录下的文件还未被跟踪
      3. `git add`, 指定所需跟踪的文件

2. 克隆现有仓库
      1. `git clone <url>`


## 记录每次更新到仓库

- 查看当前文件状态
    - `git status`

- 跟踪新文件
    - `git add <files>`
    - 使用文件或目录的路径作为参数，目录则递归跟踪该目录下的所有文件

- 暂存已修改的文件
    - 也是使用`git add <files>`

- 文件状态简要信息
    - `git status -s`
    - `?`标记表示：新增加的未跟踪文件
    - `A`标记表示：新添加到暂存区的文件
    - `M`标记表示：修改过的文件
    - 输出有两栏，左边表示暂存区的状态，右边表示工作区的状态

- 忽略文件
    - `.gitignore`列出要忽略的文件的模式
    - `.gitignore`的格式规范
        - 所有的空行，`#`号开头的行都会被忽略
        - 使用简化了的正则表达式，递归的应用到整个工作区中
        - 匹配模式可以用(`/`)开头，防止递归
        - 匹配模式可以用(`/`)结尾，指定是目录
        - 忽略指定模式以外的文件或目录，在模式前加(`!`)，取反
        - 简化的正则表达式
            - `*`匹配0个或多个任意字符
            - `[abc]`匹配任何一个列在方括号中的字符
            - `?`只匹配一个任意字符
            - `[0-9]`匹配所有0到9的数字。方括号中使用`-`分隔两字符，表示其范围内都可以匹配
            - 使用两个星号(`**`)表示匹配任意中间目录
                - 例如：`a/**/z`，可以匹配`a/z`, `a/b/z`, `a/b/c/z`等...
        - 一个仓库根目录下的`.gitignore`文件，递归的应用到整个仓库中。子目录下可以有额外的`.gitignore`文件，它的规则只作用于它所在的目录中

- 查看已暂存和未暂存的修改
    - `git diff`不带参数，查看未暂存文件更新了哪些部分
    - `git diff --staged`对比已暂存文件与最后一次提交的文件差异

- 调用其他工具比较差异
    - `git difftool`诸如使用emerge, vimdeff等工具

- 查看支持的差异比较工具
    - `git difftool --tool-help`

- 提交更新
    - `git commit`之后输入提交说明
    - `git commit -m "xxxxx"`提交信息与命令放在同一行
    - `git commit -a`加-a选项跳过使用暂存区，自动把所有以跟踪文件暂存之后一并提交

- 移除文件
    - `git rm`从已跟踪文件中移除，然后提交，该文件就不再纳入版本管理
    - `git rm -f`如果要删除之前修改过或暂存区中的文件，必须使用强制删除选项`-f`
    - `git rm --cache`不再跟踪文件，但保留文件在磁盘中，不删除
    - `git rm log/\*.log`删除log/目录下扩展名为.log的所有文件。注意`*`前的`\`，表示Git有自己的文件模式扩展匹配方式，不需要shell帮忙展开

- 重命名文件
    - `git mv file_from file_to`


## 查看提交历史

- 查看提交历史
    - `git log`
    - 显示每次提交所引入的差异
        - `git log -p -2`只显示最近两次提交所引入的差异
        - 进行代码审查，或者快速浏览提交所带来的变化时非常有用
    - 显示每次提交的简略统计信息
        - `git log --stat`
    - 将每个提交放在一行显示
        - `git log --pretty=oneline`
    - 定制显示格式
        - `git log --pretty=format:"%h - %an, %ar : %s"`
        - `%h`提交的简写哈希值
        - `%an`作者名字
        - `%ae`作者的电子邮箱
        - `%ad`作者的修订日期
        - `%ar`作者的修订日期，按多久以前的方式显示
        - `%cn`提交者的名字
        - `%ce`提交者的电子邮箱
        - `%cd`提交日期
        - `%cr`提交日期，按多久以前的方式显示
        - `%s`提交说明
    - 形象的分支，合并历史
        - `--graph`
        - `git log --pretty=format:"%h %s" --graph`
    - `git log`的常用选项
        - `-p`显示每个提交引入的差异
        - `--stat`显示每次提交的文件修改统计信息
        - `--shortstat`只显示--stat中最后的行数修改添加移除的统计信息
        - `--name-only`仅显示已修改的文件清单
        - `--name-status`显示新增，修改，删除的文件清单
        - `-relative-date`使用较短的相对时间
        - `--graph`在日志旁以ASCII图形显示分支与合并历史
        - `--pretty`使用其他格式显示历史提交信息
        - `--oneline`单行显示
    - 列出最近两周的所有提交
        - `git log --since=2.weeks`
    - 添加或删除了某一特定函数的引用的提交
        - `git log -S function_name`
    - 只关心某些文件或目录的历史提交，可以在`git log`的最后指定它们的路径，用(`--`)隔开
    - `git log`的输出选项
        - `-<n>`仅显示最近的n条提交
        - `--since`仅显示指定时间之后的提交
        - `--until`仅显示指定时间之前的提交
        - `--author`仅显示作者匹配指定字符串的提交
        - `--committer`仅显示提交者匹配指定字符串的提交
        - `--grep`仅显示提交说明中包含指定字符串的提交
        - `-S`仅显示添加或删除内容匹配指定字符串的提交
        - 示例：`git log --pretty="%h - %s" --author='xxxxx' --since="2021-10-06" --before="2021-12-08" --no-merges`
        - `-no-merges`避免显示的合并提交弄乱历史记录


## 撤销操作

- 撤销操作
    - 提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。此时可以重新提交
        - `git commit --amend`
    - 例如：提交后发现忘记了暂存某些需要的修改，进行以下操作
        ```git
        git commit -m 'initial commit'
        git add forgotten_file
        git commit --amend
        ```
        - 最终只会有一个提交结果

- 取消暂存的文件
    - `git reset HEAD <file>`
    - 例如：`git reset HEAD CONTRIBUTING.md`

- 撤销对文件的修改
    - `git checkout -- <file>`
    - 例如：`git checkout -- CONTRIBUTING.md`

- `HEAD`指向的版本就是当前版本，允许回退到历史的各个版本
    - `git reset --hard commit_id`
    - 回退到上一个版本
        - `git reset --hard HEAD^`
    - 回退到上上个版本
        - `git reset --hard HEAD^^`
    - 回退到上n个版本
        - `git reset --hard HEAD~100`，n为100，当然输入id的前几位更合适
    - 回退前可以先使用`git log`查看要回退的版本
    - 可以用`git reflog`查看历史命令，确定要回退的当前版本的未来版本的id


## 远程仓库的使用

- 查看远程仓库
    - 查看已经配置的远程仓库服务器
        - `git remote`
        - `origin`Git给克隆仓库服务器的默认名字
    - 显示需要读写远程仓库使用的Git保存的简写与其对应的URL
      - `git remote -v`

- 添加远程仓库
    - 添加一个新的远程仓库，同时指定一个方便使用的简写
          - `git remote add <shortname> <url>`
          - 然后可以在命令行中使用字符串<shortname>来代替整个URL
          - 例如：拉取仓库中有但本地没有的信息
              - `git fetch <shortname>`

- 从远程仓库获取数据
    - 访问远程仓库，从中拉取本地还没有的数据
        - `git fetch <remote>`
        - `git fetch`只会将数据下载到本地仓库，并不会自动合并或修改当前的工作
    - `git pull`可以自动抓取后合并远程分支到当前分支
    - `git clone`自动设置本地master分支跟踪克隆的远程仓库的master分支

- 推送到远程仓库
    - `git push <remote> <branch>`
    - 例如：将master分支推送到origin服务器
        - `git push origin master`

- 查看某个远程分支
    - `git remote show <remote>`
    - 例如：`git remote show origin`会列出远程仓库的URL与跟踪分支信息
    - 该命令还列出了，当在特定分支上执行`git push`会自动推送到哪个远程分支，以及执行`git pull`时，哪些本地分支可以与它跟踪的远程分支自动合并

- 重命名远程仓库
    - `git remote rename`
    - 例如：`git remote rename <old_name> <new_name>`
    - 会修改所有远程跟踪的分支名字

- 移除远程仓库
    - `git remote remove`或者`git remote rm`
    - 例如：`git remote remove xxx`
    - 所有和这个远程仓库相关的远程跟踪分支以及配置信息也会被一起删除


## 打标签

- Git可以给仓库历史中的某一个提交打上标签，表示重要
- 通常可以用来标记发布结点(v1.0, v2.0等等)
  
- 列出标签
    - `git tag`
    - 按特定模式查找标签
        - 例如：`git tag -l "1.*"`, 也可以--list

- Git支持两种标签：轻量标签(lightweight)与附注标签(annotated)
- 通常使用附注标签，附注标签包含更多信息
- 临时标签可以用轻量标签

- 创建附注标签
    - `git tag -a v1.4 -m "版本1.4"`
  
- 查看标签信息和与之对应的提交信息
    - `git show v1.4`

- 创建轻量标签
    - `git tag v1.4.1`只需要提供标签名字

- 后期打标签
    - `git tag -a v1.2 9fceb02`
    - 需要在命令的末尾指定提交的校验和(或者部分校验和)
    - 查看提交信息校验和
        - `git log --pretty=oneline`

- 默认情况下，`git push`不会传送标签到远程仓库服务器上。在创建完标签之后必须显式的推送标签到远程仓库服务器
    - `git push origin <tarname>`
    - 一次推送多个标签
        - `git push <remote> --tags`, 不会区分标签类型

- 删除标签
    - 删除本地仓库上的标签
        - `git tag -d <tagname>`
    - 删除远程仓库标签
        - `git push origin --delete <tagname>`


## Git别名

- Git只是简单的将别名替换为对应的命令
  
- 例如：`git config --global alias.ci commit`, 这样提交时可以只输入`git ci -m xxx`
  
- 为了解决取消暂存文件的易用性问题，可以添加如下别名：
    - `git config --global alias.unstage 'reset HEAD --'`
    - 于是`git unstage fileA`等价于`git reset HEAD -- fileA`
  
- 添加一个last命令，轻松查看最后一次提交：
    - `git config --global alias.last 'log -1 HEAD'`

- 如果要执行外部命令而不是Git子命令，可以在前面加(`!`)
    - 例如：`git config --global alias.visual '!gitk'`


## Git分支

- 创建分支
    - `git branch xxxxx`创建一个新分支，并不会自动切换到新分支中去

- 查看分支列表
    - `git branch`
    - (`*`)指示当前所处分支

- 查看各个分支当前所指对象
    - `git log --oneline --decorate`

- 切换分支
    - `git checkout xxxxx`
    - 分支切换会改变工作目录中的文件

- 查看分支分叉历史，输出提交历史，各个分支的指向，项目分支的分叉情况
    - `git log --oneline --decorate --graph --all`

- 创建新分支的同时切换过去
    - `git checkout -b <new_branch_name>`

- 删除分支
    - `git branch -d xxxxx`

- 合并分支
    - `git merge xxxxx`

- 任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来

- 解决所有文件里的冲突之后，使用`git add`将其标记为冲突已解决。一旦暂存这些原本有冲突的文件，Git就会将它们标记为冲突已解决

- 查看每一个分支的最后一次提交
    - `git branch -v`

- 查看哪些分支已经合并到当前分支
    - `git branch --merged`

- 查看所有包含未合并工作的分支
    - `git branch --no-merged`
  
- 删除未合并分支，丢掉那些工作
    - `git branch -D xxxxx`强制删除


## 远程分支

- 查看远程引用的完整列表
      - `git ls-remote <remote>`

- 查看远程分支的更多信息
    - `git remote show <remote>`

- origin是`git clone`时默认的远程仓库名字
- `git clone -o xxxxx`可以更改其名字

- 抓取本地没有的数据，更新本地数据库
    - `git fetch <remote>`

- 添加一个新的远程仓库引用到当前项目‘
    - `git remote add`

- 公开分享一个分支，需要将其推送到远程仓库
    - `git push <remote> <branch>`
    - 示例：
        - `git push origin serverfix:serverfix`
        - 推送本地的serverfix分支作为远程仓库的serverfix分支
        - `git push origin serverfix:xxxxx`
        - 推送本地的serverfix分支作为远程仓库的xxxxx分支

- 在远程跟踪分支之上建立一个新的用于工作的本地分支
    - `git checkout -b serverfix origin/server`
    - `git checkout --track origin/serverfix`一个快捷方式
    - 如果尝试检出的分支不存在且刚好只有一个名字与之匹配的远程分支，那么Git会创建一个跟踪分支
    - `git checkout serverfix`更快捷的方式

- 查看所有跟踪分支
    - `git branch -vv`

- 如果想要统计最新的领先与落后的数字，需要先抓取所有的远程仓库
    - `git fetch --all; git branch --vv`

- 从服务器删除分支
    - `git push origin --delete serverfix`


## 变基

- 整合来自不同分支的修改的两种方法：`merge`, `rebase`

- 提取在C4中引入的补丁和修改，然后在C3的基础上应用一次，这就是变基

- 可以使用rebase命令将提交到某一分支的所有修改到移至另一分支上

- 变基原理
    - 首先找到这两个分支的最近共同祖先，然后比对当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底，最后将之前另存为临时文件的修改依次应用到目标基底分支

- 变基使得历史提交更加整洁，提交历史是一条直线没有分叉

- 变基使用例子：
    - 检出experiment分支，然后将它变基到master分支
        - `git checkout experiment`
        - `git rebase master`
        - `git checkout master` // 回到master分支
        - `git merge experiment`    // 快进合并
    - 将client中的修改合并到主分支并发布，但暂时并不想合并server中的修改(client分支是在server分支的基础上开出来的)
        - `git rebase --onto master server client`，然后快进合并
        - 命令的意思是：取出client分支，找出它从server分支分歧之后的补丁，然后把这些补丁在master分支上重放一遍，让client看起来像是直接基于master修改一样

- 直接将主题分支变基到目标分支上
    - `git rebase <base_branch> <topic_branch>`
        - 将server分支中的修改直接变基到master分支上
            - `git rebase master server`，然后快进合并分支


## 服务器上的Git

- 远程Git仓库传输协议
    - 本地协议(Local), HTTP协议, SSH协议, Git协议

- 克隆本地版本库
    - `git clone /srv/git/project.git`  // (更快)
    - `git clone file:///srv/git/project.git`

- 增加一个本地版本库到现有的Git项目
    - `git remote add local_proj /srv/git/project.git`
    - 这样就可以通过新的远程仓库名**local_proj**像在网络上一样从远端版本库推送和拉取更新了

- 通过SSH协议克隆版本库
    - `git clone [user@]server:project.git`
    - 如果不指定可选的用户名，默认使用当前登录的名字


## 分布式Git

- 分布式工作流程
    - 集中式工作流
        - 一个中心集线器，也就是代码仓库，可以接受代码，所有人将自己的工作与之同步，若干个开发者作为结点，中心仓库的消费者与中心仓库同步
        - 如果两个开发者从中心仓库克隆代码下来，同时做了一些修改，那么只有第一个开发者可以顺利的把数据推送回共享服务器。第二个开发者在推送修改之前，必须先将第一个人的工作合并进来，这样才不会覆盖第一个人的修改
        - 使用非常广泛
    - 集成管理者工作流
        - 每个开发者拥有自己仓库的写权限和其他所有人仓库的读权限。
        - 通常有一个代表**官方**项目的权威仓库。
        - 要为这个项目做贡献，你需要从该项目克隆出一个自己的公开仓库，然后将自己的修改推送上去，接着请求官方仓库的维护者拉取更新合并到主项目
        - 维护者可以将你的仓库作为远程仓库添加进来，在本地测试你的变更，将其合并入他们的分支并推送回官方仓库
        - 工作流程如下：
            1. 项目贡献者推送到主仓库
            2. 贡献者克隆此仓库，做出修改
            3. 贡献者将数据推送到自己的公开仓库
            4. 贡献者给维护者发送邮件，请求拉取自己的更新
            5. 维护者在自己的本地仓库中，将贡献者的仓库作为远程仓库合并修改
            6. 维护者将合并后的修改推送到主仓库
        - GitHub和GitLab等集线器式(hub-base)工具最常用的工作流程
        - 优点是：你可以持续的工作，而主仓库的维护者可以随时拉取你的修改。贡献者不必等待维护者处理完提交的更新，每一方都可以按照自己的节奏工作
    - 主管与副主管工作流
        - 属于多仓库工作流程的变种，一般拥有数百位协作开发者的超大型项目才会用到这样的工作方式
        - **副主管(lieutenant)**是分别负责集成项目中的特定部分的集成管理者们
        - **主管(dictator)**是负责统筹的总集成管理者
        - 主管维护的仓库作为参考仓库，为所有协作者提供他们需要拉取的项目代码
        - 工作流程如下：
            1. 普通开发者在自己的主题分支上工作，并根据master分支进行变基，这里的master分支指的是参考仓库的master分支 
            2. 副主管将普通开发者的主题分支合并到自己的master分支
            3. 主管将所有副主管的master分支合并到自己的master分支
            4. 主管将集成后的master分支推送到参考仓库中，以便所有其他开发者以此作为基础进行变基

- 提交准则
    - Git项目提供一个文档，其中列举了关于创建提交到提交补丁的若干提示，可以在Git源代码中的`Documentation/SubmittingPatches`文件中阅读它
    - 提交不应该包含任何空白错误
        - `git diff --check`
        - 提交前使用，将会找出可能的空白错误
