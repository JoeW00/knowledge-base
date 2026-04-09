---
description: 自動找出 raw/ 下尚未 compile 的檔案並編譯（支援 PDF、docx 等）
---

請執行以下步驟：

## 階段一：盤點 raw/ 底下所有素材

1. 用 Glob 列出 `raw/` 底下所有下列副檔名的檔案（遞迴）：
   - `.md`（原生）
   - `.pdf`、`.docx`、`.pptx`、`.html`、`.txt`（需轉換）
   
2. 排除規則：
   - 跳過 `raw/repos/` 底下的檔案（repo clone 另外處理）
   - 跳過檔名以 `.` 開頭的隱藏檔
   - 若某個檔案 `X.pdf` 已經存在對應的 `X.pdf.md`，
     視為「已轉換」，後續以 `X.pdf.md` 為準

## 階段二：轉換非 md 檔案

3. 對每個非 md 檔案：
   - **PDF 論文**：優先用 `marker` 轉換為 Markdown（學術論文專用，處理雙欄排版、公式、參考文獻更佳）
     ```bash
     marker /path/to/paper.pdf --output_dir /tmp/paper.pdf.md
     ```
     轉換後讀取產出的 .md 檔案作為來源。
   - **PDF/DOCX 一般文件**：用 `markitdown` 轉換（保留表格與標題層級）
     ```bash
     markitdown /path/to/file.pdf > /tmp/file.pdf.md
   - 執行 `markitdown "原始檔路徑" > "原始檔路徑.md"`
   - 例如：`markitdown raw/papers/paper.pdf > raw/papers/paper.pdf.md`
   - 轉換失敗的檔案記錄下來，最後回報
   - 轉換成功後，在新建的 .md 檔案最上方加入 frontmatter：
     ​```yaml
     ---
     original_source: raw/papers/paper.pdf
     converted_at: {當前日期}
     ---
     ​```

## 階段三：找出未 compile 的檔案

4. 用 Glob 列出 `wiki/summaries/` 底下所有 `.md` 檔案
5. 讀取每個 summary 檔的 frontmatter `source:` 欄位，
   建立「已 compile 清單」
6. 比對出 raw/ 中所有 `.md` 檔案（包含剛轉換的）尚未出現在清單中的檔案

## 階段四：確認並執行

7. 顯示待處理清單給使用者：
   - 新轉換的檔案數量
   - 待 compile 檔案完整路徑
8. 等使用者確認後，逐一執行 /compile 流程
9. 全部完成後執行 /index

## 錯誤處理

- markitdown 失敗：記錄錯誤訊息，跳過該檔案繼續
- compile 失敗：記錄失敗檔案，繼續下一個
- 最後統一回報：成功 X 個、轉換失敗 Y 個、compile 失敗 Z 個


注意：
- 處理過程中若某個檔案 compile 失敗，記錄下來繼續下一個，
  最後統一回報失敗清單
