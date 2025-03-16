### 1. 建立一個目錄，新增 README.md 檔案，並納入 Git 管理

``` bash
mkdir my-project
cd my-project

git init

# GitHub 現在預設使用 main 作為預設分支名稱，而不是 master，可以使用以下指令修改
git branch -m master main
git branch

echo "# My Project" >> README.md
git add README.md
git commit -m "modified README.md"
```

### 2. 修改 Remote Origin 到遠端已建立的 GitHub 網址
``` bash
git remote add origin https://github.com/lws25/dkrcontainer.git
git remote -v

# 如果使用 SSH 而不是 HTTPS，請將 Remote URL 改為 SSH 格式
git remote set-url origin git@github.com:username/my-project.git

```

### 從 GitHub 抓取最新更新並合併到目前的 Branch, 
#### 使用 fetch then merge
``` bash
# 1. 確保目前 Branch 是乾淨的
git status

# 如果有未提交的變更，可以暫存
git stash
# git stash list, git stash pop, git stash clear

# 2. 抓取遠端的最新更新
git fetch origin

# 3. 合併遠端更新到目前的 Branch
git merge origin/main

# 如果有衝突，解決衝突後提交
# git add <file>
# git commit

# 4. Push 合併後的更新到遠端
git push origin main
# 或設定本地分支與遠端分支建立追蹤關係（Tracking Relationship）
#  git push --set-upstream origin main
# 修改本地分支的追蹤關係
# git branch --set-upstream-to=origin/main
# git branch -vv # 檢查本地分支的追蹤關係

```

#### 使用pull
``` bash
# git pull 是 git fetch + git merge 的組合
git pull origin main

```

#### 使用 rebase
``` bash
# 切換到 feature-branch, 將 feature-branch Rebase 到 main
git checkout feature-branch

# 執行 Rebase
git rebase main

# 如果有衝突，解決衝突後繼續
git add <file>
git rebase --continue
# git rebase --abort

# 對最近的 3 個 Commit 進行互動式 Rebase
git rebase -i HEAD~3

```

## 透過 Pull Request 提交變更
``` bash
git checkout -b feature-branch
git add .
git commit -m "Your commit message"
git push origin feature-branch

```

## 認證
### 1. 使用 Personal Access Token (PAT)
什麼是 Personal Access Token？
Personal Access Token（PAT）是一個替代密碼的令牌，用於進行 Git 操作時的認證。
你可以為不同的用途生成不同的 PAT，並設定其權限範圍（例如只允許讀取倉庫或允許寫入倉庫）。
- 如何生成 PAT？
```
1. 登入 GitHub。
2. 點擊右上角頭像，選擇 Settings。
3. 在左側選單中選擇 Developer settings。
4. 選擇 Personal access tokens，然後點擊 Generate new token。
5. 設定 Token 的名稱、有效期和權限（例如 repo 權限用於倉庫操作）。
6. 生成 Token 後，複製並保存（因為離開頁面後無法再次查看）。
```
- 如何使用 PAT？
當 Git 提示輸入密碼時，輸入你的 GitHub 帳號名稱（或 Email）作為帳號，並將 PAT 作為密碼。
例如：
```
Username: your_github_username
Password: your_personal_access_token
```

### 2. 使用 SSH 認證
``` bash
# ed25519 版本
ssh-keygen -t ed25519 -C "your_email@example.com"
# rsa 版本
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 測試 SSH 連線
# 登入 GitHub，進入 Settings > SSH and GPG keys，點擊 New SSH key。 將公鑰內容貼上並保存。
cat ~/.ssh/id_ed25519.pub
ssh -T git@github.com


```
### 定義版本號
版本號通常遵循 語意化版本控制（Semantic Versioning, SemVer） 的規範，格式為：主版本號.次版本號.修訂號（MAJOR.MINOR.PATCH）
#### 使用 Git Tag 標示版本
``` bash
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0


```
#### 建立 Release Branch
``` bash
git checkout -b release/v1.0.0
git push origin release/v1.0.0

```
#### 在 GitHub 上建立 Release
GitHub 提供了 Releases 功能，可以將 Tag 與二進制檔案（例如編譯後的程式）和發布說明結合
步驟：
```
1. 進入 GitHub 倉庫的 Releases 頁面。
2. 點擊 Draft a new release。
3. 選擇剛剛建立的 Tag（例如 v1.0.0）。
4. 填寫發布標題和說明。
5. 如果需要，上傳二進制檔案。
6. 點擊 Publish release。
```
Modified by github editor

