---
description: 將 raw/ 下的素材編譯進 wiki
---

請將以下素材編譯進 wiki：$ARGUMENTS

執行步驟：
1. 讀取指定的 raw 檔案
   - **Markdown及Text 檔案**：直接用 Read 工具讀取即可。
2. 在 `wiki/summaries/` 建立對應摘要檔案，檔名格式：
   `{原檔名}-summary.md`，內容包含：
   - frontmatter（含 source 欄位指向 raw 檔）
   - 300 字以內的摘要
   - 關鍵論點（3-5 點）
   - 原文連結或路徑
3. 從素材中抽取 3-8 個關鍵概念
4. 對每個概念：
   - 檢查 `wiki/concepts/{concept}.md` 是否存在
   - 若存在：補充新觀點，在 Related 區塊加上這篇 summary 的連結
   - 若不存在：新建概念文章，至少寫 300 字
5. 在 summary 檔案中用 [[concept]] 語法連結到所有相關概念
6. 完成後呼叫 /index 更新索引

注意：不要修改 raw/ 下的任何檔案。
