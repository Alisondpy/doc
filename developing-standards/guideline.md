前端开发引导
============

## 入职指南

  1. 工作周报模板：
  
    详情点击：[工作周报模板](./weekly-report-template.md)查看

  2. 导入 Nginx 配置:

    待定
    附 [php-fpm.conf](https://gist.github.com/allex/8efc1e13448c2498a211) for PHP-FPM fast-cgi integrations.

## 设置本地 Git 配置 `~/.gitconfig`

  **前端团队所有人员必须同步git化配**

  ```sh
  $ git config --global user.name "jiangchaoyi"
  $ git config --global user.email "jiangchaoyi@zhongzhihui.com"
  ```

  ```
  [color]
      ui = auto
  [filter "tabspace"]
      clean = expand --tabs=4 --initial
      smudge = unexpand --tabs=4 --first-only
  [alias]
      st = status --porcelain -s
      ci = commit
      br = branch
      co = checkout
      df = diff -w
      dt = difftool
      l = log --stat --format=format:'"%C(yellow)%h %C(cyan)%an %C(blue)(%cn) %C(cyan)%ai\n%C(green)%s\n%b%N"'
      lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative
      shlog = log --pretty=\"%Cgreen%h%Creset - %Cred%ci%Creset - %s (%Cred%an%Creset)\"
      fshow = !sh -c 'git show --pretty="format:" --name-only $1 | grep -v "^$" | uniq'
      vim   = !sh -c 'vim `git fshow $1`'
      mate  = !sh -c 'mate `git fshow $1`'
      subl  = !sh -c 'subl `git fshow $1`'
      wtf   = !git-wtf
      rmbranch = "!f(){ git branch -d ${1} && git push origin --delete ${1}; };f"
      rmtag = "!f(){ git tag -d ${1} && git push origin :${1}; };f"
      up = "!f(){ args=\"$@\"; [ -n \"$args\" ] || { args=\"origin `git rev-parse --abbrev-ref HEAD`\"; } ; git remote prune origin && git pull $args && git submodule update --init; git submodule foreach \"git fetch origin -p; git reset origin/master --hard\"; };f"
  [core]
      autocrlf = false
      whitespace = -trailing-space,-indent-with-non-tab,-tab-in-indent
      pager = less -R
      warnambiguousrefs = false
      excludesfile = ~/.gitignore
  [branch]
      autosetuprebase = always
  ```

##Git Installation

  ![TortoiseGit](http://download.tortoisegit.org/logo.png)

  - [Git Bash](http://git-scm.com/downloads/)
  - [TortoiseGit for Windows GUI](http://download.tortoisegit.org/tgit/1.8.13.0/)
  - [TortoiseGit 之配置密钥](http://blog.csdn.net/bendanbaichi1989/article/details/17916795)

##Sass, compass Installation

  [SCSS 初级使用](./scss-use.md)

##开发规范

  待定

##项目发布

  待定

4. 上线流程

  待定
