# uv 套件管理器使用筆記

## 什麼是 uv？
uv 是一個由 Rust 編寫的超快速 Python 套件安裝與專案管理工具。
它的目標是取代 `pip`, `pip-tools`, `virtualenv` 甚至 `poetry`。

## 為什麼選擇 uv？
1. **速度極快**：比 pip 快 10-100 倍。
2. **整合性高**：一個工具搞定 Python 套件管理、虛擬環境與套件依賴
3. **鎖定依賴(Locking)**：透過 `uv.lock` 確保所有環境安裝的版本完全一致

## 常用指令 Cheat Sheet
```bash
# 初始化新專案 (建立 pyproject.toml)
uv init

# 安裝套件 (自動更新 lock 檔)
uv add mkdocs mkdocs-material

# 安裝開發用套件 (例如測試工具)
uv add --dev pytest
```
## 執行與同步
```bash
# 在虛擬環境中執行指令 (不用手動 activate)
uv run mkdocs serve # mkdocs 測試用伺服器

# 根據 lock 檔案同步環境 (適合 CI 或剛 clone 下來時使用)
uv sync
```
