## 目錄 ##

- 這個版本是芝嘉(2019.8~2024.6之間在BML)所寫的版本，想看學長姐們在2019年前寫的版本的話，請參考[Servers Info Archive](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Servers/Servers-Info-Archive)  
- 之邑於2024.9新增「BMLab網站伺服器設定」

*  [各台Server介紹](#各台server介紹)
*  [Server開關機教學](#server開關機教學)
*  [在Server中建立新使用者](#在server中建立新使用者)
*  [Server網路斷線故障排除](#server網路斷線故障排除)
*  [BMLab網站伺服器設定](#bmlab網站伺服器設定)

## 各台Server介紹 ##
### BMLabUnit00 ###
- 目前IP位置：140.112.122.53
- 曾經的IP位置：140.112.123.123
- OS: Ubuntu 20.04.6 LTS
- 注意事項：
  - 最早購買的一台主機。
  - 物理位置在機房的第五層。
  - 曾經有一度想要當作是Database的主機，但目前沒有進度。
  - 我記得沒有外接硬碟，所以沒什麼容量可以裝東西。
  - 之前常常碰到斷線問題，解決方法請見[Server 網路斷線故障排除](#server網路斷線故障排除)。

### BMLabUnit01 ###
- 目前IP位置：140.112.122.54
- 曾經的IP位置：140.112.123.8
- OS: CentOS7
- 注意事項：
  - 第二早購買的主機。
  - 物理位置在機房的第三層。
  - 2023年7月以前是使用第四層下面那台的RAID，因為機房設計導致RAID過熱加上RAID的使用年限也很久了，所以我們又買了第四層上面那台新的RAID。目前主要是將資料存在新的RAID裡面。
  - 目前主力使用的機器之一。

### BMLabUnit02 ###
- 目前IP位置：140.112.122.55
- 曾經的IP位置：140.112.123.124
- OS: CentOS7
- 注意事項：
  - 物理位置在機房的第一層。
  - 他的RAID(還是那台是NAS我不太確定）位在第五層的上面那一台。
  - 我隱約有個印象，因為這台當初似乎是用AXC經費購買，所以老師曾經想要把這台主要拿來跑AXC資料。但是這個部分要跟老師或俊毅確認一下。
  - 目前主力使用的機器之一。

### BMLabUnit03 ###
- 目前IP位置：140.112.122.56
- 曾經的IP位置：192.168.123.137
- OS: CentOS8
- 注意事項：
  - 物理位置在機房的第二層。
  - 目前(2024）實驗室最新的一台主機。
  - 他的磁碟（主要儲存資料的空間）都已裝在機台裡面。
  - 這台當時是俊毅負責購買的。
  - 老師的定位是希望這台拿來裝舊的ADM/BLO資料。

## Server開關機教學 ##
### 前言 ###
每年醫學院都會斷電一到兩次，通常要斷電前會提前告知，你的任務就是要提前先關機，避免斷電影響到伺服器。  
印象中通常是週日斷電，所以你可以提前公告週五關機，然後強迫大家週末休息ＸＤ  
我被交接時收到的建議就是：「[Lab Wiki](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Orientation/Orientation#server%E9%96%8B%E9%97%9C%E6%A9%9F)上有寫要怎麼做。」所以我在這裡附上教學，你可以讀讀看。  
注意以下幾點已經不需要做：
- UPS系統已於2023年報廢。
- 我沒有關過四樓機房主機。

以下簡述我自己的做法，裡面會參雜Lab Wiki中的敘述。

### 關機 ###
#### 關伺服器 ####
- 【遠端，要用 `su`】請用 `root` 權限（在終端機中打 `su` 指令）登入看 `top` 才能看比較多。確認目前沒有還在跑的分析。
- 【非必要】順便做個更新，在53執行 `apt update` ；在其他伺服器執行 `yum update` 。
- 【遠端，要用 `su` 】在終端機中輸入指令 `shutdown -h now` 關閉系統。（用 `sudo` 也可以）

#### 關RAID ####
##### BMLabUnit01 (140.112.122.54) #####
- 【遠端】在伺服器上用瀏覽器連（e.g., X2GO上開firefox）10.0.0.2 出現Promise Webpam PROe，輸入[帳密](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Administration#%E5%90%84%E9%A0%85%E5%B8%B3%E5%AF%86)登入。
- 【遠端】「管理」頁籤、「關機/重新啟動（子系統）」選「關機」，鍵入"CONFIRM"確認。
- 【原作者註】如果因為任何原因無法透過10.0.0.2進入頁面關閉RAID。救急的方法是關掉伺服器之後，到機房直接手動按按鈕強制關掉RAID。
- 【芝嘉註】由於這個管理介面要使用54的firefox才能打開，我其實不太確定54關機之後要怎麼打開這個介面關RAID......我自己一直以來的做法都是關了54之後直接手動去機房關機
- 【要到機房】RAID 背後兩個"580W"下各有一個開關，關掉、聽到風扇停了就成功。

##### BMLabUnit02 (140.112.122.55) #####
- 【遠端】在伺服器上用瀏覽器連（e.g., X2GO上開firefox）192.168.11.20 出現Welcome to XS1212-D52E20，輸入帳密登入（見「各項帳密」中的"RAID @1550"）。
- 【遠端】SYSTEM SETTINGS >> Maintenance >> Reboot/Shutdown >> press Shutdown
- 【芝嘉註】上面寫的這個網址我連不上去，那我之前會跳過這兩個步驟直接去機房按power鍵。可能可以問一下俊毅怎麼連到上述的管理介面。
- 【要到機房】按power鍵（要找一下，在一可拆面板底下）。

#### 開機 ####
- 【要到機房】打開RAID電源（共有三台：54兩台、55一台），等風扇運轉趨緩即可下步驟。
- 【要到機房】打開主機電源、檢查是否有開機異常畫面。
- 【可遠端但是建議在機房】確認四台伺服器的網路狀態。
  - 檢查右上角網路是否顯示連線。
  - 輸入 `ping 8.8.8.8` 確認有正常連線。
- 【可遠端也可在機房】確認兩台主力伺服器有連接上陣列系統。
  - 打開看 `/bml/Data/` 裡面有沒有東西。
  - 【55】沒有東西的話就手動 `sudo mount /dev/sdb /home/.bml/Data/`
    - 印象中前陣子用指令的方式在55也會失敗，這個狀況要請你直接登入 `root` 的使用者介面，使用`Disk Utilities`來掛接他。
    - 詳見此[Discord thread](https://discord.com/channels/1070259860026957915/1131768343813177375) 2023.10.26的討論。
- 【遠端】再登入一次四台伺服器確定是可以連線、磁碟也都有掛接上去的狀態，沒問題就可以跟大家說開機啦然後收工！

## 在Server中建立新使用者 ##
每年都會有新加入的同學，你的任務之一是幫他們創立server account。  
比較理想的情況，我會建議是請對方在現場，直接站在你旁邊，你打一段指令，然後要設定帳號密碼的部分把鍵盤讓出來給對方打。這樣你就不需要知道對方的密碼了～

以下為步驟：
1. 跟這位同學、相關計畫的同事或是Josh確認要在哪一個server 創建帳號
2. 開Guacamole帳號
  - 之前俊毅的作風是會開啟他們自己修改密碼的權限
  - 記得勾選他們可以看到的伺服器
3. 利用下面章節的[指令](#指令)在server內建帳號
4. 教同學怎麼使用並告知注意事項
  - 因為我們主機的容量不大，所以請同學不要在桌面上或是屬於個人的資料夾內儲存任何東西，實驗室的資料都會是存在我們外接的硬碟中！

### 指令 ###
#### 創建新使用者 ####
    sudo adduser [username]  #新帳戶
    sudo passwd [username]   #帳戶密碼
    sudo usermod -a -G BMLab [username]  #將這個使用者加入BMLab的群組內
#### 修改檔案權限 ####
    sudo chgrp BMLab -R [path]  #將[path]資料夾以下的群組改為BMLab
    sudo chmod g+s [path]  #使該資料夾下新創建的檔案繼承其權限

## Server網路斷線故障排除 ##

當我們沒有辦法從自己的裝置（客戶端）連線到伺服器的時候，有時候會出現以下的錯誤訊息：

    ssh: connect to host 140.112.123.123 port 22: Operation timed out
通常這代表伺服器端目前網路連線出了狀況，如果人在實驗室的話，可以去喚醒伺服器查看一下出現了什麼問題。以下僅記錄我們過去曾經遇到過的問題還有故障排除的方式。

#### 1. 觀察目前連線狀況
先試著從 ifconfig 觀察一下我們目前有的連線介面。以BMLabUnit00(140.112.123.123)這台伺服器為例，目前(2022年3月)有的設定是：

    eno1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 140.112.123.123  netmask 255.255.255.0  broadcast 140.112.123.255
        inet6 fe80::899a:adbd:bede:f2e  prefixlen 64  scopeid 0x20<link>
        ether 90:b1:1c:0d:36:92  txqueuelen 1000  (Ethernet)
        RX packets 28712  bytes 4236201 (4.2 MB)
        RX errors 0  dropped 587  overruns 0  frame 0
        TX packets 1382  bytes 237480 (237.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device memory 0xdcd00000-dcdfffff

    eno2: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 90:b1:1c:0d:36:93  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device memory 0xdce00000-dcefffff

    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1070  bytes 90829 (90.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1070  bytes 90829 (90.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

從上面的訊息可以看到，`eno1`,`eno2`,跟`lo`是目前這台機器有開啟的介面。其中eno1目前有接一條網路線，第二行可以看到他的IP相關的設定值。

接下來，我們可以用`ping`指令去檢查目前網路的狀況。

    ping 8.8.8.8
`8.8.8.8`是Google 提供的免費ＤＮＳ伺服器的位置。如果網路狀況正常的話，接下來就會看到類似以下訊息：

    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=2.01 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=1.85 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=115 time=2.02 ms
    64 bytes from 8.8.8.8: icmp_seq=4 ttl=115 time=1.84 ms
    64 bytes from 8.8.8.8: icmp_seq=5 ttl=115 time=1.82 ms
    64 bytes from 8.8.8.8: icmp_seq=6 ttl=115 time=1.94 ms
    64 bytes from 8.8.8.8: icmp_seq=7 ttl=115 time=1.82 ms
可以按ctrl+C退出ping 測試。

反之，如果測試當下網路連線異常的話，可能會出現以下訊息：

    ping: connect: Network is unreachable

#### 2. 嘗試重開網卡
如果遇到網路連線異常的狀況，可以先嘗試將你已知負責網路連線的網卡關掉。譬如說我們已知對BMLabUnit00來說主要連線的介面是eno1，所以我可以輸入：

    sudo ifconfig eno1 down
之後再將網卡開啟：

    sudo ifconfig eno1 up

再做一次`ping`測試看網路是否恢復。


## BMLab網站伺服器設定 ##
- last modified date: 2024.9.13

### OS version ###
Ubuntu Server 24.04.1 LTS

### Profile configuration ###
- **Name:** GIBMS
- **Server name:** gibmsweb
- **Username:** gibmsadmin
- **Password:** GIBMSBM153988920

### Network Configuration ###
- **Pv4 Method:** Manual
- **Subset:** 140.112.122.0/24
- **Address:** 140.112.122.160
- **Gateway:** 140.112.122.254
- **Name servers:** 140.112.254.4, 140.112.2.2, 8.8.8.8
- **Search domains:** GIBMSWeb
- **DSN name:** gibms.mc.ntu.edu.tw
- No bonding / No proxy

### www ###
- Apache2 (version 2.4.62) is used for www service.
- SSL certificates for HTTPS is obtained for free from Let's Encrypt using Certbot, which manages automatic certificate issuance and renewal.
- Start Apache service automatically at boot `sudo systemctl enable apache2`
- /var/www/html/.htaccess
    - Redirect `gibms.mc.ntu.edu.tw` to `https://www.mc.ntu.edu.tw/gibms/Index.action`
    - Options -Indexes
- /var/www/html/bmlab/.htaccess
    - Remove `.html` from URL

### Security ###
**UFW (sudo ufw status)**
- sudo ufw default deny
- allow SSH
    - **NTU:** 140.112.0.0/16 
    - **LANs:** 192.168.0.0/16
- allow HTTP/HTTPS

**AppArmor**
- Enforce apache2

**Permission (/var/www/html)** 
- **Folders:** 755
- **Files:** 644

### Backup ###
- **crontab:** Run upgrade and backup weekly (Monday 8 a.m.)
- **script path:** /usr/local/bin/upgrade_backup.sh

