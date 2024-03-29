# Windows 11 WSL2 + Tensorflow 安裝筆記

- ## 前言  
    - 原本家裡的個人 PC，只有玩遊戲用途，原先安裝 Windows 已啟動 Secure boot，且又套用 AMD Raid 來擴充磁碟空間儲放 Steam 遊戲  
    - 近日研究有使用 Python 機器學習程式撰寫需求  
    - 在外面想透過 Tailscale 連回家網路喚醒關機的主機  
    - 以前都是在獨立的硬碟安裝 Linux 多重開機，但不在電腦前很難切換  
        - 不想破壞原先 Secure boot 設定  
        - 不想整個 Windows 推倒重裝  
    - 上述原因，興起了研究 Windows 11 WSL2 的念頭  
- ## 達成目標  
    - 在外面可透過路由器送出 wake on lan 訊號使電腦開機  
    - 電腦開機進入 Windows 11 後，可透過 tailscale 內網 ssh 連入 Windows (是的，Windows 11 可以安裝 openssh server)  
    - 連入 Windows 後，透過 wsl 命令直接進入虛擬 WSL2 環境  
    - Windows 本身安裝之最新 Nvidia Game Ready Driver，可以提供 WSL2 內部 nvidia-smi 及 cuda 存取，WSL2 不需另外安裝驅動  
    - 成功透過 Anaconda 環境執行 Tensorflow 並啟用 GPU 加速，但效能損耗不知道  
        - 也並不清楚 AMD Zen CPU 又虛擬化之後，科學運算函式庫的效能情況  
- ## 安裝步驟  
    - ### Windows 11 WSL2 安裝  
        - 前置條件：  
            - 主機板 BIOS 內虛擬化相關設定啟用  
            - Windows 11 安裝最新版 Nvidia Game Ready Driver  
            -  
              > Nvidia 一度將 WSL2 所需驅動獨立打包，後來官方有公佈說 安裝 Game Ready Driver 即可  

        - 參考 [Microsoft 官方文件](https://docs.microsoft.com/zh-tw/windows/wsl/install)，安裝 WSL2  
        - 可透過 Windows 終端 wsl 命令操作l，  
            - `wsl` 直接進入 wsl 系統命令列介面  
            - `wsl --update` 更新 kernel, Windows Update 亦會自動更新  
            - `wsl --shutdown` 可強制將 wsl2 環境進行關機  
        - 在 Microsoft 商店可選擇 Linux 發行版  
    - ### 如何從外面連接到 WSL2 系統？  
        - WSL2 系統基於 Windows Hyper-V 虛擬技術，網路連結透過虛擬 NAT  
        - 每次 WSL2 開機會自動取得 172.x.x.x 的位址  
        - WSL2 bind 之服務如 Node.js, jupyter-notebook 等，可以在 Windows 內部存取，但不設定無法透過外網直接存取  
            - 設定方式需同時考量每次 WSL2 的動態 IP 變化、Windows Defender Firewall 規則等  
            - 太過麻煩目前不考慮  
        - 目前折衷方法：  
            - **2022.9.21 更新**  
                - 原有 WSL2 因為沒有 systemd, 故無法正常啟用 tailscale  
                - 自 WSL 0.67 之後，微軟宣布開始支援 systemd, [參考文件](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/)  
                - 更新到最新版後，可以在 WSL2 內安裝 tailscale, 可在外網直接連線到 wsl2  
        - Windows 11 本身可以安裝 openssh-server  
            - [參考 Microsoft 官方文件](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui) 安裝 Windows 本身的 openssh-server ^^不是 WSL2 內部的！^^  
            - [SSH into WSL from another machine on the network](https://superuser.com/questions/1622581/ssh-into-wsl-from-another-machine-on-the-network)  
            - [設定 ssh 一連入 Windows 11 就自動連入 WSL2 方法](https://gist.github.com/mattbell87/f5bd7b78c8d0ad7f0dfc3addae4f4897)  
                - PS 會干擾 WSL2 內切換 zsh 設定  
    - ### WSL2 系統 anaconda 環境  
        - 在 Windows 11 已經安裝 Nvidia Game Ready Driver 的情況下，連入 WSL2 下 nvidia-smi 指令可以看到顯示卡和 cuda 已經呈現在 wsl2 內  
        - 跟一般 Linux 環境安裝 anaconda 步驟類似，**但不要先裝 nvidia driver**  
        - anaconda 安裝 tensorflow-gpu 有對應 Python 版本問題，目前測試 創建虛擬環境 Python 3.9 再使用 conda 安裝 tensorflow-gpu ，可在 jupyter-notebook 內成功啟用 GPU 加速  
- ## 題外話：VS Code 遠端開發  
    - [Microsoft 官方文件](https://code.visualstudio.com/docs/datascience/jupyter-notebooks)  
    - VS Code 安裝 Python, Remote-SSH, 等套件  
    - 設定 SSH 連線到 Remote 開啟資料夾後，python interpreter 可以選擇 conda 創建好的環境有安裝 jupyter 套件的環境  
    - 開新的 jupyter-notebook 即可在 VS Code 內開發

---

> 作者: Gbanyan  
> URL: https://huyan.gbanyan.net/2022/09/windows-11-wsl2--tensorflow-%E5%AE%89%E8%A3%9D%E7%AD%86%E8%A8%98/  

