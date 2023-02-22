# OPNsense 在 Proxmox VE 內安裝筆記


- ## 前言

  - 更換了軟路由硬體故順便更新了筆記
  - 此篇繼承了 [Proxmox VE + PfSense 安裝](https://huyan.gbanyan.net/2022/08/proxmox-ve--pfsense-安裝/)
    - 在 Proxmox VE 內的網路安裝架構仍然類似，皆採用半虛擬化網卡 Virtio Net 的架構
    - 經過測試，Intel J4125 搭配 Intel i225v 網卡， 不用設定網卡直通，也可以跑滿 300M / 100M
  - 更換成 OPNsense 原因有以下考量：
    - PfSense 需要訂閱 (PfSense Plus) 才會獲得較積極的更新，雖然個人用戶目前免費，但不排除未來需付費可能
    - OPNsense 的更新策略較為積極，安全性更新週期較短
    - 可安裝 Zenarmor 以及其他第三方套件來源，雖然後面還是把 Zenarmor 移除了
    - OPNsense 的 UI 比較人性化一些，可以善用搜尋快速跳到自己想要的設定欄位

- ## 初始安裝 Installation 

  - Assign the Port vmbr0 to WAN and vmbr1 to LAN (參見 PfSense 筆記內圖片)
  - Skip the lagg and vlan configuration  
  - Default account and password for enter installation  
    - account: installer  
    - pass; opnsense
  - Proxmox VE 內 VM 的設定
    - machine type: Q35
    - processors type: host
    - OS type: other
    - 其他 CPU, memory 視需求調整

- ## 設定 Configuration  
  - ### PPPoE settings with IPv6  (適用 Hinet)
  	- Fill out PPPoE info to establish connection  
  	- Tick "Use IPv4 connectivity option"  
  	- at LAN IPv6 section, IPv6 configuration type -> Track Interface, and the options below interface select "WAN"  
  	- In the Firewall options, Tick "allow IPv6"  
  	- Add IPv6 ICMP allow rule in WAN firewall rule  
  	- 如果想指定自架的 DNS (Adguard Home or Pi-Hole) 且想要應用到 IPv6:  
  		- Tick the "Manual Router announcement management"  
  		- Fill the DNS IPv4 settings in the "Router announcement"  
  		- Hinet IPv6 is Stateless (Stateless DHCPv6 + SLAAC)  
  		- Disable the DHCPv6 service in the LAN
  		- 這樣做的邏輯是，IPv6 DNS 可以只向 IPv4 位址的 DNS 伺服器請求，還是會回傳 IPv6 的解析位址
  	
  - ### 安全性設定 Security  
  	- #### System  
  	  - Disable the listen service including WebUI, ssh, Unbound on WAN surface  
  	  - Install the CrowdSec, and enable Intrusion Detection  
  	  	- Disable hardware net acceleration related "Interfaces" > "Settings"  
  	  - Configure the SSH Key, disable the password login  
  	- #### Intrusion Detection (Suricata)  (IPS/IDS)
  	  - Download rule sets based on service used  
  	  - Rule set with using sites name (p2p, Facebook, Youtube) do not apply  
  	  - ET Pro rule set need suscription  
  	- #### Crowdesc  
  	  - Connected to the cloud database to detect the attackers IPs and block  
  	  - Collection for different scenarios (windows, nginx, ...)can only be added through shell command  
  	  - The hub for adding the scenario rule [Hub |](https://hub.crowdsec.net)  
  	- #### Firehol IP list subscription  
  	  - [FireHOL Block List ( Botnets, Attacks, Malware....)](https://forum.opnsense.org/index.php?topic=17596.0)  
  	  - Follow the guide to add alias of Firehol level 2  
  	  - Add the Cron tab to update the Firewall alias  
  	- #### VLAN Configuration  (適用於建立訪客網路或者 IoT 專用網路)
  	  - Add VLAN, assign Tag, and make Proxmox VE vtnet aware vlan  
  	  - Assign DHCP server  
  	  - Add Firewall rule to make the VLAN network unable to access the LAN
  	- #### GeoIP and Ailases for Firewall Block  (如果想擋特定區域國家的話)
  	  - Register the Maxmind GeoIP database  
  	  - Follow the guide to add Firewall aliases  
  	  - Configure to block the specific countries  
  	
  - ### Cron (安全性設定完成後，記得設定各列表的更新)
  	- System and packages update  
  	- Suricata blocklist update  
  	- Firewall aliases updates (FireHol, GeoIP)  
  	
  - ### Others Packages  
  	- Netdata: 另外一種監控服務
  	- Wake-On-LAN: 遠端喚醒機器用
  	- UPNP: 有安全疑慮者慎用，給 LAN 內服務打洞用的
  	- Tailscale [OPNsense安装配置Tailscale | 鐵血男兒的BLOG](https://pfschina.org/wp/?p=9163)  
  	- Wireguard [How to Set Up WireGuard in OPNsense in 2023 - WunderTech](https://www.wundertech.net/how-to-set-up-wireguard-in-opnsense/)  

