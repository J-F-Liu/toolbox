# Git

## Tutorials

- [30 分钟 git 命令入门](https://www.jianshu.com/p/33bd9e1548e6)
- [Things About Git and Github You Need to Know as Developer](https://medium.com/swlh/things-about-git-and-github-you-need-to-know-as-developer-907baa0bed79)
- [Using submodules in Git](https://www.vogella.com/tutorials/GitSubmodules/article.html)

## Install

```
pacman -S git
yay -S gitkraken
```

## Configuration

```
git config --global --list
git config --global user.email "china.liujunfeng@gmail.com"
git config --global user.name "Junfeng Liu"
git config --global push.default simple
git config branch.master.rebase true
```

## Setup SSH Keys

```
set HOME=%HOMEPATH%
mkdir %HOME%/.ssh
git gui
Menu: Help > Show SSH Key
```

## Local Repository

```
git init
git clone REPOSITORY_URL
git clone REPOSITORY_URL --depth 10 --branch BRANCH
git clone REPOSITORY_URL --recursive 克隆嵌套的仓库
git submodule update --init --recursive  加载嵌套的仓库

git add .
git status
git commit
git commit --amend
git commit --amend --reset-author
git log
git show --quiet --oneline <commit>
git blame file_name

git stash list
git stash save
git stash pop

git log HEAD..origin --simplify-merges --reverse --format="%h %s %an %cr"

git reflog 查看所有分支的操作记录，包括已经被删除的commit记录
git fsck --lost-found
git cherry-pick 从不同的分支中捡出一个单独的commit，并把它和你当前的分支合并
```

Undo the last commit and keep the changes:

```
git reset --soft HEAD~1
```

Undo the last commit and discard the changes:

```
git reset --hard HEAD~1
```

Branch

```
git branch -v -a

git diff --check

git checkout <commit_or_branch>
git checkout -b <new_branch> <start_point>
git checkout --orphan gh-pages
git branch -d <branch_to_delete>
git branch -m <old-name> <new-name>
git merge <another_branch>
git merge --rebase <another_branch>
git merge --abort
git rebase --abort
git diff mybranch master -- myfile.cs 比较同一文件在不同分支的差异
```

Patch

```
git diff from-commit to-commit > patch.diff
git checkout taget-branch
git apply patch.diff
```

switch between multiple staging areas

```
copy %REPOROOT%\.git\index %REPOROOT%\.git\index1
set GIT_INDEX_FILE=%REPOROOT%\.git\index1
set GIT_INDEX_FILE=%REPOROOT%\.git\index
```

## Remote Repository

```
git remote -v
git remote remove origin
git remote add origin https://github.com/J-F-Liu/toolbox.git
git pull origin master
git branch --set-upstream-to=origin/master master
git push --set-upstream origin master
git push [remote_name local_branch:remote_branch]
git push --force
git fetch [remote_name]
git pull [remote_name local_branch:remote_branch]
git pull --rebase origin master
git checkout -t origin/another
git checkout another #Git versions ≥ 1.6.6
```

list all the files currently being tracked under the branch master

```
git ls-files
git ls-tree -r master --name-only

git diff-tree --no-commit-id --name-status -r <commit-ish> --find-renames
```

list files that ever existed (i.e. including deleted files):

```
git log --pretty=format: --name-only --diff-filter=A | sort - | sed '/^$/d'
git log --oneline --author=Junfeng

git reset --hard <commit>
git reset HEAD *.Cache
```

apply your local commits on top of the incoming commits, avoid merging two chains

```
git pull --rebase
```

After manually merge conflicts

```
git rebase --continue
```

delete all untracked and ignored files and folders

```
git clean -xdf
```

Archive

```
git archive -o latest.zip HEAD
```

## Best Practice

Make only one change per commit.<br/>
Making more than one change per commit is a bad thing. However, splitting a single change into more than one commit is also considered harmful.<br/>
Wear the hat of the changelog reader and try to write the most convenient sentence for him, not for you.<br/>
Write a sentence with a maximum of 50 characters, start with capital letters. Do not end sentences with periods; they are useless and even dangerous.<br/>
Adding bulleted details lines, when needed.

## Commit Message Format

```
<type>/<scope>: <subject>
\n
<body>
\n
<footer>
```

### type

Must be one of the following:

- feat: A new feature
- fix: A bug fix
- docs: Documentation only changes
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- refactor: A code change that neither fixes a bug nor adds a feature
- perf: A code change that improves performance
- test: Adding missing tests
- chore: Changes to the build process or auxiliary tools and libraries such as documentation generation

### scope

The scope could be anything that specifies the place of the commit change. For example: feature1, tests, lib, etc...

### subject

The subject contains succinct description of the change:
use the imperative, present tense: "change" not "changed" nor "changes"
don't capitalize first letter
no dot (.) at the end

### body

The body should include the motivation for the change, contrast this with previous behaviour and overall information about, why that commit matters.
Just as in the subject, use the imperative, present tense

### footer

The footer should contain any information about Breaking Changes and is also the place to reference GitHub issues that this commit closes.

## 启动 Gogs 自助式 Git 服务

```
docker pull gogs/gogs
docker run -d --name=gogs -p 10022:22 -p 10080:3000 -v /repo:/data gogs/gogs
# or use volume
docker volume create --name gogs-data
docker run -d --name=gogs -p 10022:22 -p 10080:3000 -v gogs-data:/data gogs/gogs
less /var/lib/docker/volumes/gogs-data/_data/gogs/log/gogs.log
ls /var/lib/docker/volumes/gogs-data/_data/git/gogs-repositories
```

首次运行后打开网页进行配置

- 域名: IP
- SSH 端口: 10022
- HTTP 端口: 3000
- 应用 URL: http://<IP>:10080/

开机自动运行 Gogs

```
nano /etc/systemd/system/gogs.service
```

```
[Unit]
Description=Go Git Service
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker kill gogs
ExecStartPre=-/usr/bin/docker rm gogs
ExecStart=/usr/bin/docker run -d --name=gogs -p 10022:22 -p 10080:3000 -v gogs-data:/data gogs/gogs
ExecStop=/usr/bin/docker stop gogs

[Install]
WantedBy=multi-user.target
```

```
systemctl enable gogs.service
```
