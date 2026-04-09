---
description: 重建 wiki/index.md 全域索引
---

請重新生成 `wiki/index.md`，內容包含：

1. **統計資訊**
   - concept 數量、summary 數量、exploration 數量
   - 總字數估計
   - 最近更新時間

2. **Concepts 分類**
   - 依 tag 分組列出所有 concept，每個附一句話說明
   - 使用 [[wiki-link]] 語法

3. **最近新增**
   - 依 frontmatter 的 created 欄位，列出最近 10 篇

4. **孤兒文章警告**
   - 列出沒有任何 backlink 的文章

直接覆寫 wiki/index.md。
