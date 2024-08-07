---
title: git 使用技巧
slug: git-use-skills-1kyiph
date: 2023-11-30
tags:
    - Git
categories:
    - Git
---


## git log美化输出

```bash
git log --all --decorate --oneline --graph
```

效果如下：

​![image](https://imgup.oneone.life/app/hide.php?key=djY5SkhjeXV2cEpuMWkxNUZLdTMzUE5LYmwzTmpqRDBtNUk9)​

‍

## git 拉取指定目录或文件

* 目录初始化

  ```bash
  git init
  ```
* 设置远程仓库地址

  ```bash
  git remote add -f origin <origin_url>
  ```
* ​启用`sparse checkout`​模式，允许克隆子目录

  ```bash
  git config core.sparsecheckout true
  ```
* 在` .git/info/sparse-checkout`​ 文件中指定要拉取的文件夹或文件：

  ```bash
  ## 例如要拉取远程仓库public目录下的所有文件
  echo "public/*" >> .git/info/sparse-checkout
  ```

  > 如果要拉取多个文件夹或文件，只需要在 .git/info/sparse-checkout 文件中添加多个路径即可
  >
* 开始拉取
* ```bash
  git pull origin master
  ```

‍

## git 设置全局用户名、密码、邮箱

* 设置

  ```bash
  git config --global user.name “user01”
  git config --global user.password "password"
  git config --global user.email "test@123.com"

  ```
* 删除配置

  ```bash
  git config --global --unset user.name “user01”
  git config --global --unset user.password "password"
  git config --global --unset user.email "test@123.com"
  ```

## <span style="font-weight: bold;" data-type="strong">设置</span>​<span style="font-weight: bold;" data-type="strong">`git push`</span>​<span style="font-weight: bold;" data-type="strong">和</span>​<span style="font-weight: bold;" data-type="strong">`pull`</span>​<span style="font-weight: bold;" data-type="strong">的默认远程分支</span>

```bash
git branch --set-upstream-to=origin/test test
git pull
```

## 

## 设置`git pull`​免密码

* 执行命令

  ```bash
  git config --global credential.helper store
  ```
* 命令执行后会在`家目录`​生成一个`.gitconfig`​，内容如下:

  ```git
  [credential]
  	helper = store
  ```
* 这时再执行`git pull`​，输入`用户名`​、`密码`​后，相关信息会保存到`.git-credentials`​文件中，下次再次拉取就不用输入密码啦。`.git-credentials`​文件内容结构如下：

  ```git
  http://<USERNAME>:<PASSWORD>@gitlab.com
  ```
