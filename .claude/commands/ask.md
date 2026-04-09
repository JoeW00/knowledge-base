---
description: 針對 wiki 內容進行研究型問答
---

使用者的問題：$ARGUMENTS

執行步驟：
1. 先讀 `wiki/index.md` 理解全域結構
2. 用 Grep/Glob 在 wiki/ 內搜尋相關文章
3. 讀取 3-8 篇最相關的文章
4. 若 wiki 內資訊不足，可以讀 raw/ 下對應的原始素材
5. 綜合產生答案，存到 `outputs/reports/{YYYY-MM-DD}-{slug}.md`
   - 答案必須引用具體的 wiki 文章（用 wiki-link）
   - 區分「wiki 已有的觀點」vs「你的綜合推論」
6. 完成後詢問使用者：
   「這個探索要 file 回 wiki/explorations/ 嗎？」
   若同意，複製一份到 explorations/ 並更新 index。
