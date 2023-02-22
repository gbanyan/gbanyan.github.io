# Dark Mode 黑化指南


- ## 前言  
    - 眼疾因素，開始追求特殊的黑底白字顯示模式 (Dark mode or night mode)  
    - 相信對眼睛的負擔較小，長久工作較不會累  
    - 一些研究：Effects of Dark Mode on Visual Fatigue and Acuity in Optical See-Through Head-Mounted Displays  
        - improve Visual Acuity  
        - Reduces Visual Fatigue  
        - Improves Usability and Preference in Dark Environments  
    - 也有人認為 Dark mode 讓內容更不易閱讀，反而讓眼睛負擔更重  
    - 建議視考慮個人狀況，也有做法是依照日出日落時間自動切換  
- ## 方向  
    - ### 可操作對象  
        - 螢幕：  
            - 部分螢幕內建灰階化、電子紙模式，不是黑底白字，但也可以考慮  
            - 直接購買 E-ink 螢幕，但螢幕更新率、高成本  
        - 作業系統  
            - 原生的黑底白字 UI 系統佈景，各家系統包括 iOS, Android 皆有提供，只是可能有過舊版本未能支援  
            - 無障礙輔助功能：Windows 的高對比佈景、蘋果生態系的 invert color (反相顏色)，直接強迫所有顯示黑白反相，但是有機率影響應用程式顯示，慎用。  
        - 軟體  
            - 電腦上的軟體或手機的 App，有的內建Dark mode, 會偵測系統佈景自動切換或者須由使用者手動開啟  
            - 影響範圍僅限於介面，或文件內容  
                - 程式碼編輯器：普遍支援 Dark mode, 並有不同 theme 可切換。如果沒有提供，請棄用😛。  
                - Office Word 為例，初始提供 Dark mode ，僅有介面，文件本身仍舊白得發亮。後續較新版本開始提供黑底白字的文件檢視編輯。  
                - PDF 檢視軟體，如 PDF Expert 有提供 Sepia, Night theme, 可套用至 PDF 文件本身使閱讀體驗較舒服。  
        - 網頁  
            - CSS Media query 標準支援偵測系統佈景是否啟用 Dark mode  
            - 知名網站亦提供 Dark mode 切換，如 Facebook, Twitter, Google Search...  
            - 不是所有網站都有提供 Dark mode，擴充套件可直接覆蓋樣式強迫黑底白字  
                - 所有網站皆套用：如 Dark reader, night eye, noir......  
                - Stylish 自訂網站 CSS, 進階使用者微調個別網站顯示  
    - ### 轉換流程  
        - 開啟作業系統本身的 Dark theme，通常有支援的軟體, App, 網頁就會配合修改  
        - 未配合修改的，可透過尋找軟體內設定或者 Google 關鍵字 軟體名稱 + Dark or night mode 進一步搜尋  
            - 如 Zotero 文獻管理員，透過 Google 搜尋找到有人撰寫套用 Dark mode 的擴充套件  
            - Windows 工作管理員，在 Windows 11 22H2 版後開始支援  
            - Kindle 部分電子書不支援 Dark mode, 只好啟用全系統的 invert color 反相顏色  
        - ~~透過指令介面完成所有工作，vim 是日常程式碼編輯器，GUI 滾一邊去~~  


