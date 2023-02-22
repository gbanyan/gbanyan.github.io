# Proxmox VE + PfSense 安裝


## 前言

家中的軟路由機器本只有安裝 OpenWRT, 也稱職地工作了一段時間。偶然注意到，網路高負載的情況下，記憶體佔用也才僅 8GB 記憶體的 1% 而已。硬體資源並沒有好好被妥善使用，工程魂就燃燒起來想榨乾他。

一種推薦配置是安裝 Proxmox VE 環境，再虛擬化 Router 軟體，剩下的資源就可以安裝其他客體作業系統如 Windows, Linux, 或者再裝 docker 服務。

## Proxmox VE 安裝 pfSense

安裝 Proxmox 本身的過程蠻簡單，下載 ISO 檔，燒錄到 USB 隨身碟，啟動一步一步安裝即可。在高檔硬體配置的重型伺服器，還可以考慮 ZFS 檔案系統，不過只是一台軟路由小主機就一切從簡。

下載 pfSense iso 檔，上傳到 Proxmox VE，就可以安裝了。pfSense 的安裝也有不少教學文，甚至官方也有[說明文件(Virtualizing with Proxmox® VE)](https://docs.netgate.com/pfsense/en/latest/recipes/virtualize-proxmox-ve.html)。以下僅討論個人需求和架設過程中的問題。

### 預計架構

- 軟路由機器有四個實體網路孔, enp1s0, enp2s0, enp3s0, enp4s0
- 第一個當作 WAN 孔，其餘三孔作為 LAN 孔
- Proxmox VE 可以透過 LAN 孔存取
- Proxmox VE 之下虛擬化的 pfSense 作為 Router 使用，透過 WAN 進行 PPPoE 撥號
- 家中其餘網路設備透過 LAN 得到網路存取

### 問題點

- 一開始 Proxmox VE 已經建立一個 Linux Bridge, 為 vmbr0, 關聯到 enp1s0, 並已指派原先設定的 IP 192.168.100.2
- 電腦端只能透過線路連接到 enp1s0 進行 Proxmox VE 的設定
- 將其他實體網路接孔 enp2s0, enp3s0, enp4s0 關聯到 vmbr1
- Pfsense 架設完成以後，可以透過 vmbr0 指定到 WAN, vmbr1 指定到 LAN，正常進行網路撥接及區域網路分派。但此一環境直接拿去使用，無法從任何已經連結到 LAN 的機器進入 Proxmox VE 管理介面
- 接續上述，如果要進行 Proxmox VE 的管理，要整台機器拿下來，透過 enp1s0 進行連接，十分不便

### 解決方法

經過研究半天，還曾經一度搞到中途斷電設定黨整個丟掉無法開機的窘況，要重灌 Proxmox VE 的狀況，總算搞定了… 🤷‍♂️

- 取消 vmbr0 的 預設 IP 與 Gateway，改設到 vmbr1
- 指派 vmbr1 的 IP 設定到與 Pfsense LAN 同一網段, Gateway 指向 Pfeense 192.168.1.1
	- pfSense 預設的 DHCP range 為 192.168.1.100 ~

這樣就可以透過 Pfsense 的 LAN 孔存取 Proxmox VE 了

![](/images/ProxmoxVE_network.png)

![](/images/Proxmox.drawio.png)

### 潛在問題

- Proxmox VE 主機必須要透過 pfSenese 才能連到外網
- pfSense 一旦掛掉 、遷移中停止、 或其他維護等因素，所有裝置包含 Proxmox VE 主機就有可能無法連線

## pfSense vs OpenWRT

以產品目標來了解發展脈絡，不一定要吵討論特定用途執優執劣

### pfSense

- FreeBSD 為基底，以專業防火牆目標為設計
- 防火牆管理功能一應俱全，管理監測功能完整
- 使用者介面條理分明，說明文字清楚
- Netgate 公司提供商業付費支援，完善的說明文件，且有各應用場景 scene 的相對應參考文件

### OpenWRT

- Linux 為基礎，以小型嵌入式裝置為導向，適合硬體資源相對低的家用無線路由器
- 小型輕量，除基本功能外，可以自行透過 opkg 安裝社群維護的套件
- 也有說明文件，但是沒有像 Netgate 為 pfSense 維護的文件那麼使用者導向
	- 網路相關基礎知識才能理解，或未提供細節說明，需尋找更多討論文章以尋求解答
	- 討論文章也不一定提供正確答案，需仰賴自己摸索。

我覺得兩個系統都很優秀，如果是一台無線路由器或硬體配置很低的機器要改裝，我會優先選擇 OpenWRT。但是像四核心 J1900 CPU, 記憶體裝到 8G 的機器，就會考慮裝 pfSense 來玩玩。

## Reference

- [在 UniFi Controller 5.9.29 啟用中華電信非固定制 IPv6 服務](https://www.jkg.tw/p1464/)
- [pfBlockerNG设置指南 | 鐵血男兒的BLOG](https://pfschina.org/wp/?p=6505#IPv4%E6%8A%91%E5%88%B6%E5%88%97%E8%A1%A8)
- [pfSense安装AdGuardHome | 鐵血男兒的BLOG](https://pfschina.org/wp/?p=6686)

