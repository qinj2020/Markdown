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

