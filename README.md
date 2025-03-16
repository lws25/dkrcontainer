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

### 3. 從 GitHub 抓取最新更新並合併到目前的 Branch
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

