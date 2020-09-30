# Git & GitLab Learning

## Git

### How to write a good commit message

A properly formed Git commit subject line should always be able to complete the following sentence: if applied, this commit will your subject line here. For example :

- if applied, this commit will Delete unnecessary files
- if applied, this commit will Add grep option
- if applied, this commit will Fix error when protocol is missing

It will not work for bad commit messages :

- if applied, this commit will contact page
- if applied, this commit will list of online users, some other changes because of server

### Git flow

<https://nvie.com/posts/a-successful-git-branching-model/>  
<https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow>

### Tags

選一個 tag

- [FEAT] 新增功能的變動
- [BUGFIX] 修 bug 相關的變動
- [REFACTOR] 功能不變但程式變的變動
- [TASK] 其他雜項變動
- [CHORE] 跟 code 無關的變動 例如 readme
- [STYLE] 改 coding style 相關的變動 例如縮排 命名

### pull —rebase

不要 merge remote to local

### Commit message

<https://blog.louie.lu/2017/03/21/%E5%A6%82%E4%BD%95%E5%AF%AB%E4%B8%80%E5%80%8B-git-commit-message/>
<https://gist.github.com/adeekshith/cd4c95a064977cdc6c50>

### Gitmoji

<https://gitmoji.carloscuesta.me/>

### Resource

[Online Turtorial](https://learngitbranching.js.org/)  
[Git book](https://git-scm.com/book/zh-tw/v1)  
[30 天精通 Git 版本控管](https://github.com/doggy8088/Learn-Git-in-30-days)

### Git Command

|                             |     |
| --------------------------- | --- |
| git clone [repo]            |     |
| git status                  |     |
| git remote -v               |     |
| git commit -m [comment msg] |     |
| git push origin master      |     |
| git checkout origin develop |     |

```bash
git blame 檔案名稱
```

這個指令會列出這個檔案裡的每一行是誰在什麼時間、哪一次 commit 寫進去的，想賴都賴不掉。

### How do you force a merge with Git?

[ref](https://www.quora.com/How-do-you-force-a-merge-with-Git)

#### I don’t care if there is any other conflict. My branch A will win. Always

```bash
git checkout A
git merge -s ours master
git checkout master
git merge A
```

#### It is just the File B which should win. All others should be merged as normal

```bash
git checkout A
cp B ../outsideRepository
git checkout master
git merge --no-commit --no-ff A
cp ../outsideRepsitory/B ./
# Resolve other conflicts...
git commit -m "Your merge message :-)"
```

#### Master branch? A is the new master.

```bash
git checkout A
git branch -D master #It forces to delete the master branch
git branch master #Creates a new master on current head
git checkout master
git push origin master --force
```

### Git Attributes

使用`.gitattributes`可以新增一些特殊功能，其中一項為設定`合併策略`，範例：

```git
slackbot_settings.py merge=ours
```

使用`merge=ours`請注意必須在 config 開器此功能，使用下面指令查看 config

```git
git config --global -l
```

如果沒有

```git
merge.ours.driver=true
```

請用以下指令新增：

```git
git config --global merge.ours.driver true
```

## GitLab

<https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template>
<https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/gitlab.yml.md>
<https://docs.gitlab.com/ee/raketasks/backup_restore.html>

### Changing gitlab.yml

<https://docs.gitlab.com/omnibus/settings/gitlab.yml.html>

### issue board

### CI/CD

[A beginner's guide to continuous integration](https://about.gitlab.com/2018/01/22/a-beginners-guide-to-continuous-integration/?utm_medium=social&utm_source=twitter)

[GitLab CI/CD Pipeline Configuration Reference](https://docs.gitlab.com/ee/ci/yaml/README.html#includetemplate)

### SSH

### Backup and Restore

<https://docs.gitlab.com/ee/raketasks/backup_restore.html>

將備份丟到 dropbox，記得

```bash
cd /etc/gitlab
sudo vim gitlab.rb
# gitlab_rails['manage_backup_path'] = false
# gitlab_rails['backup_path'] = "/home/kai/Dropbox/gitlab_backup"
# gitlab_rails['backup_archive_permissions'] = 0777
gitlab-ctl reconfigure
sudo crontab -e
24 * * * * gitlab-rake gitlab:backup:create
```

不幸版本不合你需要：

[Downgrade](https://docs.gitlab.com/omnibus/update/README.html#downgrading)  
[特定版本](https://packages.gitlab.com/gitlab/)

### CI template: CS build and robocopy

```yml
stages:
    - build
    - deploy

build:
    stage: build
    tags:
        - windows
    only:
        - branches
    script:
        - chcp 65001
        - echo on
        - '%NUGET% restore ScupioMonitor.sln'
        - '%MSBUILD% ScupioMonitor.sln /p:Configuration=Debug'
    artifacts:
        name: '%CI_JOB_NAME%-%CI_COMMIT_REF_SLUG%'
        expire_in: 1 week
        paths:
            - HealthChecker/bin/Debug/*.exe
            - HealthChecker/bin/Debug/*.dll

deploy:
    stage: deploy
    tags:
        - windows
    only:
        - branches
    script:
        - chcp 65001
        - echo on
        - net use \\n046\c$\ScupioTasks /user:administrator %DEPLOY_PASS%
        # https://stackoverflow.com/questions/44504795/how-to-stop-robocopy-from-exiting-the-build
        - (robocopy HealthChecker/bin/Debug/ //n046/c$/ScupioTasks) ^& IF %ERRORLEVEL% LSS 8 SET ERRORLEVEL = 0
```
