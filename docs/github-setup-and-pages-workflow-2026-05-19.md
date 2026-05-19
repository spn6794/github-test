# GitHub 連接與 GitHub Pages 部署流程紀錄

日期：2026-05-19

## 目標

把 Codex 與 GitHub 連接起來，完成 GitHub CLI 登入、建立測試 repo、啟用 GitHub Pages，並整理日後網頁 push 與部署流程。

## 已完成事項

- 讀取並依照 `02-連接-GitHub.md` 懶人包流程執行。
- 設定 Git 使用者資訊：
  - `user.name`: Stephen
  - `user.email`: yudalai@gmail.com
- 確認 Git 安裝位置：`C:\Program Files\Git\cmd\git.exe`。
- 確認 GitHub CLI 安裝位置：`C:\Program Files (x86)\GitHub CLI\gh.exe`。
- 將 Git 與 GitHub CLI 加入 Windows 使用者 PATH。
- 使用 GitHub CLI 的 device-code 流程授權 GitHub。
- 建立測試 repo：`spn6794/github-test`。
- 建立測試頁：`index.html`，內容顯示「Hello！GitHub 連接成功！」。
- 推送到 GitHub：`https://github.com/spn6794/github-test`。
- 啟用 GitHub Pages：`https://spn6794.github.io/github-test/`。
- GitHub Pages 建置狀態曾確認為 `built`。
- 使用者選擇保留測試 repo。

## 遇到的問題與處理

### `git` / `gh` 一開始抓不到

Codex 當下的 PATH 尚未包含 Git 與 GitHub CLI。處理方式：

- 先用完整路徑執行。
- 再把 Git 與 GitHub CLI 安裝資料夾加入使用者 PATH。
- 重開 Codex 後，之後應可直接使用 `git` 與 `gh`。

### GitHub CLI 登入需要手動授權

使用 `gh auth login --web --git-protocol https` 後，GitHub 會要求到 device 頁面輸入驗證碼並授權。

### Git 顯示 dubious ownership

建立測試 repo 時，Git 因 Codex 沙盒使用者與 Windows 使用者不同，出現 dubious ownership。處理方式：

```powershell
git config --global --add safe.directory "C:/Users/User/我的雲端硬碟/2026codeX/GitHub安裝/github-test"
```

### GitHub Pages API 參數格式

第一次啟用 Pages 時，GitHub 預設找 `gh-pages` 分支而失敗。正確做法是用巢狀參數指定 `main` 分支與根目錄：

```powershell
gh api repos/spn6794/github-test/pages -X POST -F "source[branch]=main" -F "source[path]=/"
```

### 2026-05-19 後續檢查

後續再次檢查時，`gh auth status` 曾顯示 token invalid。若之後 GitHub CLI 操作失敗，需要重新登入：

```powershell
gh auth login -h github.com --web --git-protocol https
```

## 日後新網站上線流程

在新網站資料夾中，至少要有 `index.html`。第一次上線：

```powershell
git init
git branch -M main
git add .
git commit -m "Initial website"
gh repo create 網站名稱 --public --source=. --push
gh api repos/spn6794/網站名稱/pages -X POST -F "source[branch]=main" -F "source[path]=/"
```

部署後網址格式：

```text
https://spn6794.github.io/網站名稱/
```

## 日後更新已上線網站

已經部署過的網站，修改檔案後只需要：

```powershell
git add .
git commit -m "Update website"
git push
```

GitHub Pages 會自動重新部署，通常等待約 1 分鐘。

## 之後可直接對 Codex 說

- 「幫我把這個網頁推到 GitHub 上線，repo 名稱叫 `xxx`。」
- 「幫我更新這個 GitHub Pages 網站。」
- 「幫我產生 QR Code 給學生掃。」

Codex 可協助完成：建立 repo、提交、推送、啟用 Pages、回報網址。
