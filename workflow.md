# Workflow 設計、執行檢查紀錄與反思

## 一、Loop / Graph 流程設計

本專案採用「雙迴圈（Dual-Loop）驗證架構」，將流程拆為 **Wiki 建置迴圈** 與 **簡報生成與查核迴圈**，避免 AI 直接從零生成簡報造成幻覺或脫離資料來源。

```mermaid
flowchart TD
    A[1. 取得真實資料來源<br/>5 篇規則/論文/戰術教材] --> B[2. Ingest 寫入 sources/<br/>單篇摘要與來源連結]
    B --> C[3. 提煉至 concepts/<br/>跨篇整合與新手轉譯]
    C --> D{檢查 1: Wiki 品質查核<br/>是否涵蓋 5 篇？術語是否過難？}
    D -- 不通過：補齊或改寫 --> C
    D -- 通過 --> E[4. 規劃教學簡報大綱<br/>限制僅讀取 concepts/ 與 sources/]
    E --> F[5. 生成單檔 HTML 簡報<br/>每頁標註 Ref 來源]
    F --> G{檢查 2: 簡報與 Wiki 核對<br/>有無幻覺？版面與換頁是否正常？}
    G -- 發現問題：修正內容或 CSS --> F
    G -- 通過 --> H[6. 輸出 index.html 並部署 GitHub Pages]