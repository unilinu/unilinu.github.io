## [LF/CRLF](http://kuanghy.github.io/2017/03/19/git-lf-or-crlf)
```sh
git config --global core.autocrlf input # 提交时转换为LF，检出时不转换
git config --global core.safecrlf true # 拒绝提交包含混合换行符的文件
```
## [.gitignore](https://blog.csdn.net/mingjie1212/article/details/51689606)
```sh
git rm -r --cached .  # 清除缓存
git add . # 重新trace file
git commit -m "update .gitignore" # 提交和注释
git push origin master # 可选，如果需要同步到remote上的话
```
## [reset remote](https://segmentfault.com/q/1010000002898735)
```sh
git revert <commitId> # 未尝试过
```

```sh
git reset --soft <commitId>
git stas # 暂存修改
git push --force # 强制push,远程的最新commit被删除
git stash pop # 释放暂存的修改，开始修改代码
git add . -> git commit -m "massage" -> git push
```

## git push with user:pass
```sh
// origin=git@github.com:unilinu/repo.git
git push https://username:password@github/unilinu/repo.git --all
git pull -r https://username:password@github/unilinu/repo.git
```