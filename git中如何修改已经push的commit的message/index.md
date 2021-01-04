# Git中如何修改已经push的commit的message


# 背景

当你用终端操作 git，可能会出现 commit 的 message 乱码或者写错的情况，但是如果你已经将代码 push 到了 github 中，那么修改 message 就是一个非常麻烦的操作。

# 需要用到的命令

```linux
$ git log
$ git rebase -i HEAD~5
$ git commit --amend
$ git rebase --continue
$ git push -f
```

**注：**
在修复历史 commit message 的时候，请确保当前分支是最新代码，
且已经提交了所有本地修改。

# 修改步骤

1. 使用 `git log` 查看日志。
   如图，提交记录会根据时间倒序展示。
   ![git log](git-log.png)
2. 使用`git rebase -i HEAD~2` ,这里 1 可以换成别的数字，意思是显示最近 2 个提交记录。

   ```
   pick 1d316b0 1
   pick f429786 2
   pick 880cfbc 3
   pick c55cf56 4
   pick d10fd07 5
   ```

   - 最左侧是命令（command）
     包括：<br>

     - p, pick = use commit
     - r, reword = use commit, but edit the commit message
     - e, edit = use commit, but stop for amending
     - s, squash = use commit, but meld into previous commit
     - f, fixup = like “squash”, but discard this commit’s log message
     - x, exec = run command (the rest of the line) using shell
     - d, drop = remove commit

   - 中间是 commit id
   - 最右一列是 commit message

3. 假设我们要修改最后一条 message，就要把相应的 pick 改为 edit，保存并退出。
   ```
   edit 1d316b0 1
   pick f429786 2
   pick 880cfbc 3
   pick c55cf56 4
   pick d10fd07 5
   ```
4. 随后轮流使用 `git commit --amend` 和 `git rebase --continue` 修改每个 edit 的 commit。
   保存完了之后，git 的分支就会发生改变，
   从原来的 master 改成了我们第一个 edit 的 commit id。
   下面我们在这个 commit id 所示的分支上，执行`git commit --amend`,此时可以使用 vim 或者其他文本编辑器修改 message。
   保存后使用 `git rebase --continue`，若修改多个 message，只需重复几次。

# 使用 git push 强制更新远程服务器

`git push -f` 强制修改

