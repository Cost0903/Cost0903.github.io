# GitHub Actions 自動化部署實戰

這篇筆記紀錄了如何使用 Github Actions 將 MkDocs 網站自動化部署到 Github Pages

## 為什麼要自動化 ?
身為一名後端工程師，減少重複性的手動工作 (Manual Toil) 是 DevOps 的核心精神。<br>
透過 CI/CD 我們可以：<br>
* **避免人為失誤**：防止忘記編譯、指令輸入錯誤、自動化測試。<br>
* **專注內容**：寫完 Code `git push` 即可，無須手動執行後續動作。<br>
* **版本控制**：部署流程本身也被寫成程式碼 (Infrastructure as Code) 管理。<br>

## 實作步驟

### 1. 準備 Workflow 設定檔
在專案的根目錄建立 `.github/workflows/ci.yml`

### 2. Workflow 程式碼解析
我們使用標準的 Python 環境來執行 MkDocs 指令

```yaml
name: Publish docs via GitHub Pages
on:
  push: # 執行 push 且 branch 為 main 時觸發
    branches:
      - main
permissions:
  contents: write # 賦予 Agent write 權限

jobs:
  deploy:
    runs-on: ubuntu-latest # 使用 ubuntu 虛擬機
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: 3.x

      - name: Install dependencies
        run: pip install mkdocs-material

      - name: Deploy to Github Pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: mkdocs gh-deploy --force # 部署 mkdocs
```

如果使用 uv 作為我們的套件管理器
```yaml
name: Publish docs via Github Actions
on:
  push:
    branches:
      - main # 主要分支
permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Install uv
        # 使用官方 Action 安裝 uv
        uses: astral-sh/setup-uv@v5
        with:
          version: "latest"

      - name: Set up Python
        run: uv python install

      - name: Install dependencies
        # uv sync 會讀取 uv.lock 並安裝完全一致的套件版本
        run: uv sync --all-extras --dev

      - name: Deploy to Github Pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: uv run mkdocs gh-deploy --force
```