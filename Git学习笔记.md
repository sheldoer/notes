## 2021.2.2  git bash学习笔记

### 1.1基础操作

```
git status 查看当前状态


mkdir test 创建test文件夹


touch test.py 创建test.py文件
```

#### 初始化Git

```
cd test 切换到test文件夹


Git init 初始化仓库
```

### 1.2添加文件

```
git add a1.py


git commit -m “描述”
```

### 1.3修改文件

```
Vi a1.py 完成后按esc键，输入:wq退出


git add a1.py 


git commit -m “描述”
```

### 1.4删除文件

```
rm -rf test.py


git rm test.py

git commit -m “描述”
git push 传送到远程仓库
```

