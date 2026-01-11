# SDD 設計資產展示平台

這是一個用於軟體詳細設計 (SDD) 流程的 HTML 設計展示平台，透過 Vercel 按需求分支部署，讓 PM 可以在獨立分支上修改設計，工程師可以參考對應分支的設計進行開發。

## 🎯 專案目標

- **PM**：在需求專屬分支上修改和調整設計，互不干擾
- **工程師**：參考對應需求分支的設計進行開發
- **團隊協作**：透過 master 分支的索引頁面，快速找到各需求的 Vercel 部署連結

## 🏗️ 架構設計

### Master 分支
- 只包含**需求清單索引頁面**
- 列出所有需求及其 Vercel 部署連結
- 不包含實際設計檔案

### 功能分支（feature/REQ-XXXXXX）
- 每個需求一個獨立分支
- 包含該需求的所有設計檔案
- Vercel 自動部署，生成專屬 URL

## 📁 專案結構

### Master 分支結構
```
sdd-design-assets/ (master)
├── public/
│   └── index.html        # README.md
├── vercel.json
├── package.json
├── .gitignore
└── README.md
```

### 功能分支結構（例：feature/REQ-000001）
```
sdd-design-assets/ (feature/REQ-000001)
├── public/
│   ├── index.html        # README.md
│   ├── home/
│   │   ├── code.html
│   │   └── screen.png
│   ├── login/
│   │   ├── code.html
│   │   └── screen.png
│   └── register/
│       ├── code.html
│       └── screen.png
├── vercel.json
├── package.json
└── README.md
```

## 🚀 快速開始

### 1. 檢視現有需求

訪問 master 分支的部署：
```
https://sdd-design-assets.vercel.app
```

點擊需求卡片即可查看該需求的設計。

### 2. 本地開發

```bash
# 切換到需求分支
git checkout feature/REQ-000001

# 安裝 serve（如果尚未安裝）
npm install -g serve

# 執行本地伺服器
npm run dev

# 或直接使用 serve
serve public
```

本地預覽網址：http://localhost:3000

### 3. 部署到 Vercel

#### 首次設定（透過 GitHub 整合）

1. 將專案推送到 GitHub
2. 前往 [Vercel Dashboard](https://vercel.com/dashboard)
3. 點擊 "Add New..." → "Project"
4. 選擇你的 GitHub repository
5. 設定如下：
   - **Framework Preset**: Other
   - **Root Directory**: `./`
   - **Output Directory**: `public`
   - **Build Command**: 留空
   - **Install Command**: 留空
6. **重要**：啟用所有分支的自動部署
   - 在 Project Settings → Git
   - 勾選 "Automatically deploy all branches"
7. 點擊 "Deploy"

設定完成後，每個分支推送都會自動觸發部署。

## 🔄 工作流程

### 新增需求設計

```bash
# 1. 從 master 建立新的需求分支
git checkout master
git pull
git checkout -b feature/REQ-000002

# 2. 建立設計檔案結構
mkdir -p public/dashboard public/settings
# 在各資料夾中放入 code.html 和 screen.png

# 3. 建立該需求的導航頁面
# 編輯 public/index.html，列出該需求的所有頁面

# 4. 提交並推送
git add .
git commit -m "feat: 新增 REQ-000002 設計資產"
git push -u origin feature/REQ-000002
```

Vercel 會自動部署，URL 格式：
```
https://sdd-design-assets-git-feature-req-000002-popopoponys-projects.vercel.app
```

### PM 修改現有設計

```bash
# 1. 切換到需求分支
git checkout feature/REQ-000001
git pull

# 2. 修改設計檔案
# 編輯 public/*/code.html 或 screen.png

# 3. 提交變更
git add .
git commit -m "feat: 更新登入頁面設計"
git push
```

Vercel 會自動重新部署該分支。

### 設計完成後（可選）

標記為已完成：

```bash
git checkout feature/REQ-000001
git tag -a v1.0-completed -m "設計已完成並交付開發"
git push origin v1.0-completed
```

## 🌐 Vercel URL 說明

### Master 分支（README 說明頁）
```
https://sdd-design-assets.vercel.app
```

### 功能分支（設計展示）
```
# URL 格式
https://sdd-design-assets-git-[分支名]-popopoponys-projects.vercel.app

# 範例：REQ-000001
https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app
```

### 訪問設計頁面

每個需求分支的設計頁面路徑：
```
# 主畫面
https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/home/code.html

# 登入頁面
https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/login/code.html

# 註冊頁面
https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/register/code.html
```

**注意**：需要完整路徑包含 `.html` 副檔名。

## 📝 命名規範

### 分支命名
- 格式：`feature/REQ-XXXXXX`
- 範例：`feature/REQ-000001`, `feature/REQ-000042`

### 頁面資料夾
- 使用小寫 (kebab-case)
- 範例：`home`, `login`, `register`, `user-profile`, `dashboard`

### 檔案命名
- HTML 檔案：`code.html`
- 截圖檔案：`screen.png`

## 💡 最佳實踐

1. **一需求一分支**：每個需求都在獨立的 feature/REQ-XXXXXX 分支
2. **Master 只有索引**：master 分支只維護需求清單，不放設計檔案
3. **及時更新索引**：新需求建立後，記得在 master 的 index.html 加入連結
4. **保持分支活躍**：設計完成後不要刪除分支，保留供日後參考
5. **截圖同步更新**：修改 HTML 後記得更新對應的截圖

## 🛠 常見問題

### Q: 如何查看某個需求的設計？

A:
1. 從 Vercel Dashboard 找到對應分支的 URL
2. 或直接訪問：`https://sdd-design-assets-git-feature-req-XXXXXX-popopoponys-projects.vercel.app/[page-name]/code.html`
3. 或本地：`git checkout feature/REQ-000001 && npm run dev`

### Q: 我要修改 REQ-000001 的設計，怎麼做？

A:
```bash
git checkout feature/REQ-000001
git pull
# 修改檔案
git add .
git commit -m "feat: 更新設計"
git push
```

Vercel 會自動重新部署這個分支。

### Q: 如何新增新需求？

A:
1. 建立新分支：`git checkout -b feature/REQ-000XXX`
2. 在 `public/` 中加入設計資料夾和檔案
3. 推送分支：`git push -u origin feature/REQ-000XXX`
4. Vercel 會自動部署該分支

### Q: 本地如何預覽？

A:
```bash
# 先切換到要預覽的分支
git checkout feature/REQ-000001
# 啟動本地伺服器
npm run dev
```

### Q: 分支會不會太多？

A: 這正是架構的優點！每個需求獨立，互不影響。Vercel 會為每個分支保持獨立的部署，方便隨時查看歷史設計。

## 🔧 技術說明

- **靜態網站**：純 HTML + Tailwind CSS，無需建置步驟
- **自動部署**：Git push 觸發 Vercel 自動部署
- **分支隔離**：每個分支有獨立的部署 URL
- **零維護成本**：Vercel 免費方案足夠使用

## 📊 目前部署狀態

### 已部署的需求

**REQ-000001 - 失智症檢測小幫手**
- 分支：`feature/REQ-000001`
- 根路徑：https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app
- 設計頁面：
  - [主畫面 (Home)](https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/home/code.html)
  - [登入頁面 (Login)](https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/login/code.html)
  - [註冊頁面 (Register)](https://sdd-design-assets-git-feature-req-000001-popopoponys-projects.vercel.app/register/code.html)

### Master 分支
- URL：https://sdd-design-assets.vercel.app
- 內容：README 說明文件

## 📄 授權

MIT License

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request！
