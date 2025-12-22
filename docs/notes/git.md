# Git 實用技巧與疑難排解

## 1. 處理 `site/` 資料夾誤上傳
如果不小心將編譯後的 `site/` 資料夾或是一些敏感資訊推送到 Github
可以使用以下步驟在「保留本機檔案」的情況下，移除遠端的追蹤。

```bash
# 1. 從 Git 索引中移除 (不刪除實體檔案)
git rm -r --cached site/

# 2. 更新 .gitignore 確保未來不在追蹤
echo "site/" >> .gitignore

# 3. 提交變更
git commit -m "Stop tracking site/ folder"
git push
```