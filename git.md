# 使用代理
```
ALL_PROXY="socks5://192.168.31.131:3080"  git clone XXX
```
# 拉取远程分支
``` shell
git fetch origin <分支名>
git checkout -b <本地分支名> origin/<远程分支名>
```

# format-patch

``` shell
git format-patch HEAD^ # 最近1次commit
git format-patch HEAD^^  # 最近2次commit
# ... 以此类推

git format-patch <r1>..<r2> # 生成包含<r1>至<r2>的patch

git format-patch -1 <r1> # 生成单个commit的patch

git format-patch <r1> 生成自<1>到当前的patch (不包括此commit)

git format-patch --root <r1> # 生成从root到r1的patch

```


## 应用patch

git apply --stat 0001-limit-log-function.patch # 查看patch的情况
git apply --check 0001-limit-log-function.patch # 检查patch是否能够打上，如果没有任何输出，则说明无冲突，可以打上
(注：git apply是另外一种打patch的命令，其与git am的区别是，git apply并不会将commit message等打上去，打完patch后需要重新git add和git commit，而git am会直接将patch的所有信息打上去，而且不用重新git add和git commit,author也是patch的author而不是打patch的人)
git am 0001-limit-log-function.patch # 将名字为0001-limit-log-function.patch的patch打上
git am --signoff 0001-limit-log-function.patch # 添加-s或者--signoff，还可以把自己的名字添加为signed off by信息，作用是注明打patch的人是谁，因为有时打patch的人并不是patch的作者
git am ~/patch-set/*.patch　# 将路径~/patch-set/*.patch 按照先后顺序打上
git am --abort              # 当git am失败时，用以将已经在am过程中打上的patch废弃掉(比如有三个patch，打到第三个patch时有冲突，那么这条命令会把打上的前两个patch丢弃掉，返回没有打patch的状态)
git am --resolved           #当git am失败，解决完冲突后，这条命令会接着打patch


**如果应用补丁时提示trailing whitespace, 可以使用--whitespace=warn或者--whitespace=fix**
