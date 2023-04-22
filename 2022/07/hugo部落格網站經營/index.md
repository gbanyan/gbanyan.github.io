# Hugo部落格網站經營


網址：[https://huyan.gbanyan.net](https://huyan.gbanyan.net)

## 未來計畫

- 增加 Cover image
- 增加 favicon
- 設定 Social metadata
- 設定 Comment 功能
- 設定 Google Analytics
- 加上 Cloudflare Proxy

[Ref](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/)

## 原則

- 化繁為簡，以內容為重
- 以 Markdown Note 為基礎，盡可能降低發布成本，從 Obsidian 轉錄方便為佳
- 將更新維護考慮進去，重視資料標籤及最後更新日期資訊
- 標籤使用節制，與 Obsidian 共用同一系統

## 規劃

### 網站資訊

- Hugo static site generater
- Hugo theme: [Papermod](https://adityatelange.github.io/hugo-PaperMod/)
	- 簡潔
	- Light/ Dark mode switch
	- 支援部分 Social Metadata
	- 支援 TOC
- 網址結構 URL Structure：`/:year/:month/:title/`

### 目錄層級

- About
- tags
- archives
- search

### Social Site Link

- Twitter
- RSS

 - 未來可能考慮: Github, ...etc

## 維護流程

### Hugo 與佈景更新

- Hugo through macOS brew package manager
- Papermod theme 更新:

```bash
cd themes/PaperMod 
git pull
``` 

### 發新文章

- `hugo new posts/title.md`
- 填入 Markdown 內容，修改標籤及 draft 狀態

---

> 作者: Gbanyan  
> URL: https://huyan.gbanyan.net/2022/07/hugo%E9%83%A8%E8%90%BD%E6%A0%BC%E7%B6%B2%E7%AB%99%E7%B6%93%E7%87%9F/  

