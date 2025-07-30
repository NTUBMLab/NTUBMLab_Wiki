# 目錄

- [前言](#前言)  
  - [一、文件目的與來源說明](#一文件目的與來源說明)  
  - [二、實驗室設備與架構概述](#二實驗室設備與架構概述)  

- [第一章：分析用伺服器管理](#第一章分析用伺服器管理)  
  - [一、伺服器定位](#一伺服器定位)  
  - [二、連外主機與 Guacamole](#二連外主機與-guacamole)  
  - [三、Linux 維護常用指令](#三linux-維護常用指令)  
  - [四、新進人員帳號設定](#四新進人員帳號設定)  
  - [五、MATLAB 安裝與更新](#五matlab-安裝與更新)  
  - [六、伺服器開關機](#六伺服器開關機)  

- [第二章：網站主機管理](#第二章網站主機管理)  
  - [一、主機基本資訊](#一主機基本資訊)  
  - [二、網路設定](#二網路設定)  
  - [三、網頁伺服器 Apache](#三網頁伺服器-apache)  
  - [四、網頁設定檔 .htaccess](#四網頁設定檔-htaccess)  
  - [五、安全性設定](#五安全性設定)  
  - [六、自動備份與更新](#六自動備份與更新)  
  - [七、Git clone (遠程編輯實驗室網頁HTML檔案)](#七git-clone-遠程編輯實驗室網頁html檔案)  

- [第三章：實驗室網路架構](#第三章實驗室網路架構)  
  - [一、路由器 Router](#一路由器-router)  
  - [二、交換器 Switch](#二交換器-switch)  
- [附錄：問題排解紀錄](#附錄問題排解紀錄)  
  - [MATLAB 啟動錯誤（Error 5201）](#matlab-啟動錯誤error-5201)  
  - [.55 上 Guacamole 登入後立即被登出](#55-上-guacamole-登入後立即被登出)  
  - [.56 SSH 連線異常（與 virbr0 衝突）](#56-ssh-連線異常與-virbr0-衝突)  


# 前言

## 一、文件目的與來源說明：

本文件為 BMLAB 實驗室伺服器、網站主機與網路相關設定之交接與操作指南。內容主要改編自芝嘉的交接筆記，並整合 Lab Wiki 中之資訊，由之邑於 2025 年 4 月彙整與補充更新。除了此份文件也可參考 [GitLab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Servers/BML-Server-Notes) 上的內容作補充，惟部分內容已久未更新，需自行判斷其適用性。

## 二、實驗室設備與架構概述：

BMLAB 實驗室目前主要資訊設備如下：

- 四台分析用伺服器（設置於 12 樓 1232 室）  
- 一台連外伺服器（設置於醫學院四樓機房）  
- 一台網站伺服器（設置於 12 樓 1245 室）


| IP 位址         | 舊 IP             | 用途     | GPU | 記憶體   | 儲存空間 | 處理器數量 | 作業系統                |
|----------------|------------------|----------|-----|----------|----------|------------|-------------------------|
| 140.112.122.53 | 140.112.123.123  | 分析主機 | 否  | 約 47 GB | 1.7 TB   | 32         | Ubuntu 20.04.6 LTS      |
| 140.112.122.54 | 140.112.123.8    | 分析主機 | 是  | 約 94 GB | 37 TB    | 16         | CentOS 7                |
| 140.112.122.55 | 140.112.123.124  | 分析主機 | 是  | 約 92 GB | 32 TB    | 32         | CentOS 7                |
| 140.112.122.56 | 192.168.123.137  | 分析主機 | 否  | 約 62 GB | 30 TB    | 40         | CentOS 8                |
| 140.112.121.218 | -                | 連外主機 | -  | -        | -        | -          | CentOS 7                |
| 140.112.122.160 | -                | 網站主機 | -  | -        | -        | -          | Ubuntu 24.04.2 LTS      |

# 第一章：分析用伺服器管理

BMLAB 實驗室目前使用之分析用伺服器設置於 **腦心所 1232 室（人類核心實驗室）**。各台機器於機架上皆設有標籤，便於辨識與管理。

---

### 維修聯絡資訊

如伺服器硬體發生異常，請聯繫以下單位協助處理：

- 晟得電腦：<sunward.cs@msa.hinet.net>  
- 晟得電腦李金興經理：<jslee652123@gmail.com>  
- 若 `.54` 的 RAID 發生故障，請聯繫 RAID 維修廠商 **喬鼎資訊 Alan Chu**


---

#### KVM 控制說明

實驗室所有分析主機共用一組 **螢幕、滑鼠與鍵盤**，透過 KVM 切換器進行控制。

| KVM 按鍵編號 | 對應伺服器 IP       |
|--------------|---------------------|
| 1            | 140.112.122.53      |
| 2            | 140.112.122.54      |
| 3            | 140.112.122.55      |
| 4            | 140.112.122.56      |

## 一、伺服器定位

- **140.112.122.53**  
  最早期部署的分析主機，核心數較多但儲存空間較小，當前使用頻率較低。

- **140.112.122.54**  
  儲存空間最大、具備 GPU，儘管核心數較少，仍為目前主要使用的分析主機之一。  
  *備註：本機配備兩組 RAID。2023 年 7 月以前主要使用第一組 RAID，但因過熱與設備老化問題，已改採新購之第二組 RAID 為主要儲存裝置。*

- **140.112.122.55**  
  具備大容量儲存與 GPU（效能略低於 54 號機），為目前另一主要使用之分析主機。  
  *備註：本機配備單一 RAID 系統。*

- **140.112.122.56**  
  擁有最多邏輯核心及大容量內接硬碟，目前亦為主要使用主機之一。


## 二、連外主機與 Guacamole

- **遠端介面網址**：`http://140.112.121.218:8080/guacamole/#/`
- **主機資訊**：部署於醫學院四樓機房，並與醫學院資訊組每年續租機櫃空間。
- **功能說明**：透過 Apache Guacamole® 提供圖形化不斷線遠端桌面介面，允許使用者於校外網域進行分析主機的連線與操作，為 ssh 之外的替代方式，適用於 GUI 分析需求。


## 三、Linux 維護常用指令

> Linux 基礎教學補充資源：[鳥哥 - Linux 基礎學習篇](https://linux.vbird.org/linux_basic)

### 權限與群組管理

* `sudo chgrp BMLab -R [path]` # 將指定資料夾及其下所有檔案的群組設定為 BMLab
* `sudo chmod g+s [path]` # 使新建檔案繼承目錄群組權限


### 系統與資源監控

* `history`  #檢視歷史指令記錄
* `df -h` # 顯示磁碟使用情況（含單位）
* `kill` # 終止指定程序
* `top` # 即時顯示系統資源使用情況（Ctrl+C 離開）


### top 進階說明

* **load average**：顯示系統平均負載，三個數值分別為過去 1、5、15 分鐘的平均進程數量。
	* 計算方式：活躍進程 / 邏輯 CPU 數量
		* 小於 3：系統運行良好
		* 大於 5：系統負載偏高，可能過度擁擠
* **st (Steal Time)**：虛擬機等待實體 CPU 的時間百分比
	* 正常應為 0%
	* 若達 50%，表示 CPU 資源分配不足
	* 若達 100%，表示完全無法取得 CPU，需檢查虛擬化環境
* top 使用快捷鍵：
	* `M`：依記憶體使用量排序
	* `P`：依 CPU 使用率排序
	* `o` → 輸入 `S=status`：依程式狀態篩選
* 程式狀態類型（Status）：
	* `R`（Running）：執行中
	* `S`（Sleep）：可喚醒之待命狀態
	* `D`（Uninterruptible Sleep）：等待 I/O 無法中斷
	* `T`（Stopped）：暫停或被除錯中
	* `Z`（Zombie）：殭屍行程，已結束但未被清除


## 四、新進人員帳號設定

### 帳號設定流程

1. 與該同學、合作計畫成員或 Josh 確認需開設帳號的伺服器。
2. 更新 Google 雲端硬碟中的 **BML Server ID List** 檔案，確保帳號資訊同步記錄。
3. 建立 Guacamole 帳號，並勾選其可存取之伺服器。
4. 提醒使用者：伺服器儲存空間有限，請避免儲存與分析無關之資料，並定期清理暫存檔案。
5. 協助說明以下工具與指令的基本使用方式：
   - `ssh`（遠端登入）
   - `sftp`（檔案傳輸）
   - Guacamole（圖形化遠端桌面）
   - MATLAB（指令啟動與簡要操作）


### 建立帳號相關指令

* `sudo adduser [username]` # 建立新使用者帳號
* `sudo passwd [username]` # 設定使用者密碼
* `sudo usermod -a -G BMLab [username]` # 將使用者加入 BMLab 群組
* `sudo adduser/usermod -c "comment" [username]` # 附註真實姓名（方便識別與管理）


## 五、MATLAB 安裝與更新

>  MATLAB 每年會於3 月（a 版）與9 月（b 版）進行例行更新。是否升級至最新版本可與老師討論後決定。

---

如於安裝或使用過程中遇到問題，請洽鈦思科技支援：

- 鈦思科技客服專線：**(03) 550-5590**  
- 教育業務：Chang Anna（anna.chang@terasoft.com.tw）  
- 工程師支援：Yen Jialing（jialing.yen@terasoft.com.tw）  
- 教育推廣：Shie Adelaide（adelaide.shie@terasoft.com.tw）  


### 更新步驟

1. 前往 [MathWorks 官方網站](https://www.mathworks.com/)，使用實驗室帳號登入（帳密請見 [GitLab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Administration#各項帳密)）。
2. 點選 **My Account**，下載最新版本 MATLAB 安裝檔，Label 請選擇 _MATLAB (individual)_。
3. 前往下載目錄，解壓縮後執行安裝指令：`sudo ./install`
4. 安裝過程中：
	* 勾選安裝所有 Toolbox
	* License Label 選擇 _MATLAB (individual)_
5. 安裝完成後，確認 MATLAB 是否已建立執行捷徑：`ls /usr/local/bin | grep matlab`
6. 若 `/usr/local/MATLAB` 目錄中仍保留舊版本（例如 R2023b），可依需求刪除：`sudo rm -rf /usr/local/MATLAB/Rxxxxx`


## 六、伺服器開關機

> 每年醫學院會安排 1～2 次機房斷電（通常於週末進行），為避免突發斷電導致伺服器或儲存裝置損毀，請於週五提前完成以下關機步驟，並於電力恢復後依序開機。

---

### 關機流程

1. **卸載 RAID 裝置**
	- 使用 `umount` 指令卸載 RAID
	- 務必記下掛載點，開機後需重新掛接

2. **關閉主機**
	- 使用指令：`sudo shutdown -h now`
   
3. **關閉 RAID 電源**
	- 前往機房，按下 RAID 裝置的實體電源鍵
	- 聽見風扇停止運轉即表示成功關閉
	- **注意**：部分主機電源鍵藏在機殼背面，請小心拉出機架（動作需緩慢、避免扯線），建議使用手機拍照確認開關位置。
	- **提示**：`.55` 的 RAID 開關位置位於靠牆側中偏左的黑色長方形區塊上方，需長按 3～5 秒才會關機


### 開機流程

1. **開啟 RAID 裝置**
	- 先開 RAID，等待風扇聲平穩後再開主機

2. **開啟主機**
	- 按下實體電源鍵
	- 建議先將 KVM 螢幕切換至該台主機（特別是 `.54` 與 `.55`）觀察啟動過程

3. **掛載 RAID**
	- 使用先前記錄的掛載點執行 mount 作業
	- `.55` 若掛載異常，請使用 root 帳號登入 GUI，開啟 Disk Utilities 進行手動掛接（參考[Discord](https://discord.com/channels/1070259860026957915/1131768343813177375) 討論紀錄）

4. **測試網路是否連線成功**
	- `ping 8.8.8.8`


# 第二章：網站主機管理

> 伺服器基礎知識可參考補充資源：[鳥哥的 Linux 私房菜 - 伺服器篇](https://linux.vbird.org/linux_server/)

---

## 一、主機基本資訊

- **主機位置**：行政主機左側機架，與行政主機共用螢幕、滑鼠與鍵盤，透過 KVM 切換操作
- **作業系統**：Ubuntu Server 24.04.1 LTS
- **Profile 名稱**：`GIBMS`
- **主機名稱**：`gibmsweb`
- **登入帳密**：見 [GitLab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Servers/BML-Server-Notes#bmlab%E7%B6%B2%E7%AB%99%E4%BC%BA%E6%9C%8D%E5%99%A8%E8%A8%AD%E5%AE%9A)


## 二、網路設定

- **IPv4 Method**：Manual（靜態 IP）
- **IP 位址**：140.112.122.160  
- **子網域（Subnet）**：140.112.122.0/24  
- **預設閘道（Gateway）**：140.112.122.254  
- **DNS 伺服器**：140.112.254.4、140.112.2.2、8.8.8.8  
- **搜尋網域（Search Domain）**：GIBMSWeb  
- **DNS 名稱**：`gibms.mc.ntu.edu.tw`  
- **其他設定**：無 Bonding、無 Proxy


## 三、網頁伺服器 Apache

- **軟體版本**：Apache2 version 2.4.62
- **功能**：提供網站服務（www）
- **HTTPS 憑證**：使用 Let's Encrypt 搭配 Certbot 取得，支援自動續期
- **啟動設定**：`sudo systemctl enable apache2`


## 四、網頁設定檔 .htaccess

* `/var/www/html/.htaccess`：
	* 將 `gibms.mc.ntu.edu.tw` 重導向至 `https://www.mc.ntu.edu.tw/gibms/Index.action`
* 停用目錄瀏覽：`Options -Indexes`
* `/var/www/html/bmlab/.htaccess`：
	* 移除 `.html` 副檔名於 URL 中顯示
	


## 五、安全性設定

* UFW 防火牆設定：


```
sudo ufw status
sudo ufw default deny
sudo ufw allow from 140.112.0.0/16      # NTU
sudo ufw allow from 192.168.0.0/16      # LANs
sudo ufw allow http
sudo ufw allow https
```

* AppArmor 安全模組：
	* 已啟用 Enforce 模式以保護 Apache 執行環境

* 網站目錄權限設定：
	* 路徑：`/var/www/html`
	* 資料夾權限：`755`
	* 檔案權限：`644`


## 六、自動備份與更新

* 每週排程透過Crontab服務自動執行系統更新與網站備份
	* 排程時間：每週一上午 08:00
	* 執行腳本路徑：`/usr/local/bin/upgrade_backup.sh`



## 七、Git clone (遠程編輯實驗室網頁HTML檔案) ##

若要編輯BMLab網站內容，原先必須在行政主機旁邊的伺服器主機或以SSH到140.112.122.160，在沒有圖形使用者介面的狀況下編輯html檔案，不僅不直觀而且也相較不方便。藉由Git clone的Cloning repository能做到允許編輯人在個人電腦本地編輯網頁html檔案，上傳至雲端伺服器再由伺服器主機同步以反映更新內容。

#### 下載並安裝github desktop ####

* 前往github官網下載：https://desktop.github.com/
* 安裝完成後點右上角 Sign in to GitHub.com，輸入帳號密碼完成登入。


#### 同步儲存庫到個人電腦 #### 

* 在 GitHub Desktop 介面右上角點選 File > Clone Repository > URL
* 複製[BML-Website](https://github.com/NTUBMLab/BML-Website)的github repository URL: https://github.com/NTUBMLab/BML-Website.git 至URL slot
* 在下方路徑決定同步下來的Repository檔案要下載到本地電腦的哪個位址。(預設地址為C:\Users\使用者\Documents\GitHub)
* 點擊 Clone，等待下載完成。


#### 在個人電腦編輯儲存庫 #### 

* 整個儲存庫檔案會為預設位在C:\Users\使用者\Documents\GitHub，可以在個人電腦直接編輯html或整個Repository
** 可以使用VS Code或直接用記事本編輯html檔案，包含其他圖片影片連結。


#### 將本地編輯Push至線上Repository #### 

* 回到GitHub Desktop，你應該能看到目前做出的編輯顯示於左側Changes，確定編輯沒問題之後在下面的Summary與Description簡單輸入編輯原因後即可按下**Commit to main**
* 最後按下**Fetch origin**即可Push編輯至雲端
* 至[BML-Website](https://github.com/NTUBMLab/BML-Website)檢查編輯是否應用。


#### 在實驗室伺服器Pull同步檔案 #### 

* 一般來說請聯絡William來幫你進行(XD)，不過這裡還是會記錄使用方法。
	* 在終端機執行: cd /var/www/html
	* 在終端機執行: sudo git clone https://github.com/NTUBMLab/BML-Website.git (可能會要你輸入SUDO密碼)
	* 顯示”Username for https://github.com”: 輸入GITHUB的帳號
	* 顯示”Password for https://github.com”: 輸入GITHUB的密碼
		* 注意，若你的GITHUB是用第三方軟件登入(如GOOGLE登入)，登入密碼會較麻煩，請見乙。
		* 若無法登入或第三方登入，請用我的帳密:
		* 帳號輸入: william215463497
		* 密碼輸入: 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
	* 完成了，到官網檢查修訂是否應用。
	* 請特別注意檔名與路徑，在伺服器主機上的路徑應該為: /var/www/html/bmlab (注意，若儲存庫名稱不是bmlab請將檔名更改為bmlab)


# 第三章：實驗室網路架構

如遇無法排解之網路連線問題，請聯繫醫學院資訊組協助處理。

---

## 一、路由器 Router

- **品牌**：NETGEAR
- **位置**：位於實驗室門口冰箱櫃上方
- **管理介面 IP**：`192.168.1.1`
- **登入帳號密碼**：見 [GitLab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Administration#各項帳密)

- **Wi-Fi 網路區分**：
  - 分為 **實驗室內部使用** 與 **訪客/受試者使用**
  - 出於資安考量，請勿混用或交叉使用
  - 相關 SSID 與密碼資訊也請見 [GitLab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Administration#各項帳密)


## 二、交換器 Switch

- **位置**：資訊箱內（實驗室門口處三個鐵櫃中，最下方一櫃，貼有「資訊箱」標示）  
	- **注意**：上方兩個鐵櫃請勿隨意開啟！

- **交換器配置**：
  - **小型交換器**：分配至**Josh 辦公室**
  - **大型交換器**：分配至**各實驗室桌位**

- **實驗室網路主線說明**：
  - 有兩條來自醫學院的主網路線（黃橘色）：
    - 第一條：直接接至小型交換器，提供一組 IP
    - 第二條：先透過灰色網線接至路由器，再接至大型交換器，提供另一組 IP

- **大型交換器網路孔編號對應**：
  - 每個實驗桌位對應一組有線網路孔
  - 線材上標示之編號即對應座位位置，便於管理與除錯
    
- **實驗室網路示意圖**：


```
            醫學院主幹網路線（黃橘） ×2
                      │
      ┌───────────────┼───────────────┐
      │                               │
      ▼                               ▼

小型交換器（老師辦公室專用）     灰線 → 路由器（NETGEAR）
[提供 1 個 IP]                          │
                                       ▼
                          灰線 → 大型交換器（實驗室座位使用）
                                       │
             ┌─────────────────────────┼─────────────────────────┐......
             │                         │                         │
          LAN #01                   LAN #02                   LAN #03
         （座位 1）                  （座位 2）                 （座位 3）
```

- **故障排查建議**：
  - 如遇無法連線情況，請**先確認交換器上對應網路孔是否亮燈**  
    （無亮燈可能代表線材脫落或主線連接異常）
  


# 附錄：問題排解紀錄

## MATLAB 啟動錯誤（Error 5201）

### 問題描述

* 在伺服器上啟動 MATLAB 時，可能會遇到錯誤訊息 `error 5201`，導致無法成功登入。


### 解決方式

* 請執行以下指令以清除相關連線暫存：
`rm -rf ~/.MathWorks/ServiceHost ~/.MATLABConnector`
* 執行後重新開啟 MATLAB 並登入，即可正常使用


## .55 上 Guacamole 登入後立即被登出

### 問題描述

* 伺服器 .55 發生使用者登入 Guacamole 後立即被登出的情況。
* 參考 [Discord 討論紀錄](https://discord.com/channels/1070259860026957915/118988994015109125)


### 解決方式

* 請使用者依序執行以下指令建立正確的 GUI 啟動環境：


```
touch ~/.xsession
echo "/usr/bin/dbus-launch /usr/bin/startxfce4" > ~/.xsession
chmod +x ~/.xsession
```

## .56 SSH 連線異常（與 virbr0 衝突）

### 問題描述

* 伺服器 `.56` 出現從內部 IP（`140.112.121.122`）連線時無法 SSH 的狀況。
* 通常發生於伺服器重新開機後。
* 原因為虛擬網路裝置 `virbr0` 的 IP 配置與實驗室子網段發生衝突，導致連線中斷。


### 解決方式

* 請停用 `virbr0` 虛擬網路介面：`sudo ifdown virbr0`
* 預期回應： `Connection 'virbr0' successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/2)`