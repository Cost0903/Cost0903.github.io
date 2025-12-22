# GitHub Actions 自動化部署實戰

這篇筆記紀錄了如何使用 Github Actions 將 MkDocs 網站自動化部署到 Github Pages。

## 為什麼要自動化 ?
身為一名後端工程師~~(以及一名懶人)~~，減少重複性的手動工作 (Manual Toil) 是 DevOps 的核心精神。
透過 CI/CD 我們可以：
* **避免人為失誤**：防止忘記編譯、指令輸入錯誤、自動化測試。
* **專注內容**：寫完 Code `git push` 即可，無須手動執行後續動作。
* **版本控制**：部署流程本身也被寫成程式碼 (Infrastructure as Code) 管理。

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
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force # 部署 mkdocs
```