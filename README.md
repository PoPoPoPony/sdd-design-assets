# SDD 設計資產展示平台

這是一個用於軟體詳細設計 (SDD) 流程的 HTML 設計展示平台，透過 Vercel 按需求分支部署，讓 PM 可以在獨立分支上修改設計，工程師可以參考對應分支的設計進行開發。

## 🎯 專案目標

- **PM**：在需求專屬分支上修改和調整設計，互不干擾
- **工程師**：參考對應需求分支的設計進行開發
- **團隊協作**：每個需求獨立分支，透過 Vercel 自動部署

## 🏗️ 架構設計

### Master 分支
- **配置基礎分支**：只包含專案配置檔案
- 不包含任何展示頁面或設計檔案
- 所有功能分支從這裡 checkout

### 功能分支（feature/REQ-XXXXXX）
- 每個需求一個獨立分支
- 包含該需求的所有設計檔案（HTML + 截圖）
- Vercel 自動部署，生成專屬 URL

## 📁 專案結構

### Master 分支結構
```
sdd-design-assets/ (master)
├── vercel.json          # Vercel 部署設定
├── package.json         # 專案設定檔
├── .gitignore          # Git 忽略設定
└── README.md           # 說明文件
```

### 功能分支結構（例：feature/REQ-000001）
```
sdd-design-assets/ (feature/REQ-000001)
├── public/
│   ├── Index/
│   │   ├── code.html     # 主畫面 HTML
│   │   └── screen.png    # 主畫面截圖
│   ├── Login/
│   │   ├── code.html     # 登入頁面 HTML
│   │   └── screen.png    # 登入頁面截圖
│   └── Register/
│       ├── code.html     # 註冊頁面 HTML
│       └── screen.png    # 註冊頁面截圖
├── vercel.json
├── package.json
└── README.md
```

## 🚀 快速開始

### 1. 本地開發

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

本地預覽網址：
- http://localhost:3000/Index/code.html
- http://localhost:3000/Login/code.html
- http://localhost:3000/Register/code.html

### 2. 部署到 Vercel

#### 首次設定（透過 GitHub 整合）

1. 將專案推送到 GitHub
2. 前往 [Vercel Dashboard](https://vercel.com/dashboard)
3. 點擊 "Add New..." → "Project"
4. 選擇你的 GitHub repository
5. 設定如下：
   - **Framework Preset**: Other
   - **Root Directory**: `./`
   - **Build Command**: 留空
   - **Output Directory**: `public`
   - **Install Command**: 留空
6. 點擊 "Deploy"
7. **重要**：部署後進入 Project Settings → Git
   - 勾選 "Automatically deploy all branches pushed to Git repository"

完成後，每個分支推送都會自動觸發部署。

## 🔄 工作流程

### 新增需求設計

```bash
# 1. 從 master 建立新的需求分支
git checkout master
git pull
git checkout -b feature/REQ-000002

# 2. 建立設計檔案結構
mkdir -p public/Dashboard public/Settings
# 在各資料夾中放入 code.html 和 screen.png

# 3. 提交並推送
git add .
git commit -m "feat: 新增 REQ-000002 設計資產"
git push -u origin feature/REQ-000002
```

Vercel 會自動部署，URL 格式：
```
https://sdd-design-assets-git-feature-req-000002-[你的帳號].vercel.app
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

### 工程師參考設計

直接訪問對應需求分支的 Vercel URL：

```
https://sdd-design-assets-git-feature-req-000001-[帳號].vercel.app/Login/code.html
```

## 🌐 Vercel URL 說明

### Master 分支
```
https://sdd-design-assets.vercel.app
```
（無展示內容，僅配置基礎）

### 功能分支
```
# 格式
https://sdd-design-assets-git-[分支名]-[帳號].vercel.app

# 範例
https://sdd-design-assets-git-feature-req-000001-popopopony.vercel.app
```

### 訪問設計頁面
```
[分支 URL]/Index/code.html
[分支 URL]/Login/code.html
[分支 URL]/Register/code.html
```

## 📋 如何找到各需求的 URL？

### 方法 1：Vercel Dashboard
1. 前往 [Vercel Dashboard](https://vercel.com/dashboard)
2. 選擇專案 `sdd-design-assets`
3. 在 "Deployments" 頁籤查看所有分支的部署
4. 每個分支都有專屬的 URL

### 方法 2：GitHub
1. 前往 GitHub repository
2. 切換到對應的分支（例：feature/REQ-000001）
3. README 中通常會記錄該分支的 Vercel URL

### 方法 3：本地查詢
```bash
# 列出所有分支
git branch -a

# 切換到需求分支
git checkout feature/REQ-000001

# 啟動本地預覽
npm run dev
```

## 📝 命名規範

### 分支命名
- 格式：`feature/REQ-XXXXXX`
- 範例：`feature/REQ-000001`, `feature/REQ-000042`

### 頁面資料夾
- 使用 PascalCase
- 範例：`Index`, `Login`, `Register`, `UserProfile`, `Dashboard`

### 檔案命名
- HTML 檔案：`code.html`
- 截圖檔案：`screen.png`

## 💡 最佳實踐

1. **一需求一分支**：每個需求都在獨立的 feature/REQ-XXXXXX 分支
2. **Master 只有配置**：master 分支只維護專案配置，不放設計檔案
3. **分支永久保留**：設計完成後不要刪除分支，保留供日後參考
4. **及時更新截圖**：修改 HTML 後記得更新對應的截圖
5. **使用 Git Tag**：設計完成後可以打 tag 標記：
   ```bash
   git tag -a REQ-000001-v1.0 -m "設計完成並交付開發"
   git push origin REQ-000001-v1.0
   ```

## 🛠 常見問題

### Q: 如何查看某個需求的設計？

A:
1. 從 Vercel Dashboard 找到對應分支的 URL
2. 或者本地：`git checkout feature/REQ-000001 && npm run dev`

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

Vercel 會自動重新部署。

### Q: 如何新增需求？

A:
```bash
git checkout master
git pull
git checkout -b feature/REQ-000XXX
# 建立設計檔案
git add .
git commit -m "feat: 新增 REQ-000XXX 設計資產"
git push -u origin feature/REQ-000XXX
```

### Q: Master 分支訪問會看到什麼？

A: Master 分支沒有 public 資料夾，訪問 https://sdd-design-assets.vercel.app 可能會顯示 404 或空白頁。這是正常的，因為 master 只是配置基礎。

### Q: 本地如何預覽？

A:
```bash
# 切換到要預覽的分支
git checkout feature/REQ-000001
# 啟動本地伺服器
npm run dev
# 訪問 http://localhost:3000/Index/code.html
```

### Q: 設計完成後要合併回 master 嗎？

A: **不用**！功能分支應該永久保留，不要合併回 master。設計完成後可以打 tag 標記即可。

## 📊 專案狀態

### 已部署的需求
- ✅ REQ-000001：失智症檢測小幫手（Login, Register, Index）

### Vercel 部署資訊
- 專案 URL: https://sdd-design-assets.vercel.app
- 自動部署：已啟用所有分支

## 🔧 技術說明

- **靜態網站**：純 HTML + Tailwind CSS，無需建置步驟
- **自動部署**：Git push 觸發 Vercel 自動部署
- **分支隔離**：每個分支有獨立的部署 URL
- **零維護成本**：Vercel 免費方案足夠使用

## 📄 授權

MIT License

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request！
