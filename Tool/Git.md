# Git

## Install
```
pacman -S git
packer -S gitkraken
```

## Configuration
```
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
git clone https://github.com/J-F-Liu/toolbox.git

git add .
git status
git commit
git commit --amend
git commit --amend --reset-author

git stash list
git stash save
git stash pop

git reflog 查看所有分支的操作记录，包括已经被删除的commit记录
git fsck --lost-found
git cherry-pick 从不同的分支中捡出一个单独的commit，并把它和你当前的分支合并
```

## Remote Repository
```
git remote -v
git remote remove origin
git remote add origin https://github.com/J-F-Liu/toolbox.git
git branch --set-upstream-to=origin/master master
git push --set-upstream origin master
git push
git pull
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

git reset --hard
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

Branch
```
git branch -v

git diff --check

git checkout <commit>
git checkout master
git checkout -b <new_branch> <start_point>
```

switch between multiple staging areas
```
copy %REPOROOT%\.git\index %REPOROOT%\.git\index1
set GIT_INDEX_FILE=%REPOROOT%\.git\index1
set GIT_INDEX_FILE=%REPOROOT%\.git\index
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

## 启动Gogs自助式Git服务
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
- SSH端口: 10022
- HTTP端口: 3000
- 应用 URL: http://<IP>:10080/

开机自动运行Gogs
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

- [30 分钟 git 命令入门](http://www.w3ctrain.com/2016/06/26/learn-git-in-30-minutes/)
