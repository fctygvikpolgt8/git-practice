# Git 版本管理学习笔记

## Git 的三个区域

```
工作区 (Working Directory)  →  暂存区 (Staging Area)  →  仓库 (Repository)
     你编辑的文件                git add 后的文件           git commit 后永久保存
```

---

## 基本操作

### 1. 初始化仓库

```bash
git init
```

在当前目录创建 Git 仓库，会生成一个隐藏的 `.git` 文件夹。

---

### 2. 查看状态

```bash
git status
```

常用输出及含义：
- `Untracked files` — 新文件，Git 还没有跟踪
- `Changes not staged for commit` — 已跟踪的文件有修改，但还没暂存
- `Changes to be committed` — 文件已暂存，等待提交
- `nothing to commit, working tree clean` — 没有更改，工作区干净

---

### 3. 暂存文件

```bash
# 暂存单个文件
git add hello.txt

# 暂存所有更改
git add .
```

---

### 4. 提交更改

```bash
git commit -m "提交说明"
```

提交说明应该简洁描述这次改了什么，比如：
- `git commit -m "添加登录功能"`
- `git commit -m "修复首页显示bug"`

---

### 5. 查看修改内容

```bash
git diff
```

显示工作区和暂存区的差异。输出中：
- `+` 开头的行 = 新增的内容
- `-` 开头的行 = 删除的内容

---

### 6. 查看提交历史

```bash
# 完整日志
git log

# 简洁模式（推荐）
git log --oneline

# 带分支图的简洁模式
git log --oneline --graph --all
```

---

## 分支操作

### 7. 创建分支

```bash
git branch feature-add-name
```

### 8. 切换分支

```bash
git checkout feature-add-name
```

### 9. 创建并切换分支（简写）

```bash
git checkout -b feature-add-name
```

### 10. 查看所有分支

```bash
git branch
```

当前分支前面会有 `*` 号标记。

### 11. 合并分支

```bash
# 先切回主分支
git checkout master

# 再合并功能分支
git merge feature-add-name
```

### 12. 删除已合并的分支

```bash
git branch -d feature-add-name
```

---

## 日常工作流程

```
1. 编辑文件
2. git status          # 看看改了什么
3. git diff            # 查看具体改动
4. git add <文件>       # 暂存想提交的文件
5. git commit -m "说明" # 提交
6. git log --oneline   # 确认提交记录
```

---

## 在 VSCode 中的对应操作

| 操作 | VSCode 位置 |
|---|---|
| 查看更改 | 左侧第三个图标「源代码管理」 |
| 查看文件差异 | 在源代码管理面板点击文件名 |
| 暂存文件 | 文件旁点击 `+` 号 |
| 提交 | 输入框写信息，点击 `✓` |
| 切换分支 | 左下角点击分支名 |
| 查看当前分支 | 左下角显示 |

---

## 术语速查

| 英文 | 中文 | 说明 |
|---|---|---|
| Repository (repo) | 仓库 | Git 项目的数据库 |
| Commit | 提交 | 一次保存的快照 |
| Branch | 分支 | 独立的开发线路 |
| Merge | 合并 | 把分支的改动合并回来 |
| Stage / Add | 暂存 | 准备好要提交的改动 |
| Working Directory | 工作区 | 你正在编辑的文件 |
| Stash | 暂存现场 | 临时保存当前改动 |
| Remote | 远程仓库 | 服务器上的仓库（如 GitHub） |
| Origin | 远程默认名 | 远程仓库的默认别名 |

---

## 进阶操作

### 13. 撤销工作区的修改

```bash
git checkout -- <文件>
# 或新版写法
git restore <文件>
```

丢弃工作区的修改，恢复到上次提交的状态。**不可逆，谨慎使用。**

---

### 14. 撤销暂存

```bash
git restore --staged <文件>
```

把文件从暂存区移回工作区，修改内容不会丢失。

---

### 15. 撤销提交（git reset）

```bash
# --soft：撤销提交，修改保留在暂存区
git reset --soft HEAD~1

# --mixed：撤销提交，修改保留在工作区（默认）
git reset --mixed HEAD~1

# --hard：撤销提交，删除所有修改（危险！）
git reset --hard HEAD~1
```

`HEAD~1` 表示回退一个提交，`HEAD~2` 回退两个，以此类推。

---

### 16. 暂存工作现场（git stash）

```bash
git stash          # 临时保存当前改动
git stash pop      # 恢复并删除记录
git stash list     # 查看所有暂存记录
git stash apply    # 恢复但保留记录
git stash drop     # 删除某条记录
```

使用场景：正在开发功能A，突然需要去修功能B的bug。

---

### 17. 远程仓库操作

```bash
# 添加远程仓库
git remote add origin https://github.com/用户名/仓库名.git

# 第一次推送（-u 设置默认关联）
git push -u origin master

# 之后的日常推送
git push

# 从远程拉取更新
git pull

# 克隆别人的仓库
git clone https://github.com/用户名/仓库名.git
```

---

---

### 18. 解决合并冲突

合并时如果两个分支修改了同一位置，会产生冲突：

```
<<<<<<< HEAD
当前分支的内容
=======
要合并的分支的内容
>>>>>>> feature-xxx
```

解决步骤：
1. 打开冲突文件，找到 `<<<<<<<` 标记
2. 决定保留哪个版本（或合并两者）
3. 删掉 `<<<<<<<`、`=======`、`>>>>>>>` 标记
4. `git add <文件>` → `git commit`

---

### 19. .gitignore

创建 `.gitignore` 文件，列出不需要跟踪的文件：

```gitignore
# 依赖目录
node_modules/

# 环境变量（含密码，不要提交！）
.env

# 日志文件
*.log

# 系统文件
.DS_Store
Thumbs.db

# 编辑器配置
.vscode/
.idea/
```

**注意：** 如果文件已经被 Git 跟踪过，需要先取消跟踪：
```bash
git rm --cached <文件>
```

---

## 进阶日常工作流程

```
1. git pull             # 先拉取最新代码
2. git checkout -b xxx  # 创建功能分支
3. 编辑文件...
4. git add / git commit # 提交
5. git push -u origin xxx  # 推送到远程
6. 在 GitHub 上发起 Pull Request
7. 合并后删除分支
```
