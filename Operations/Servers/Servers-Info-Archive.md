---
title: Servers Info Archive
---


## 目錄 ##
這個版本是學長姐們在2019年前所寫的版本，想看新的資料的話，請參考[BML Server Notes](https://gitlab.com/brain-and-mind-lab/Wiki/bmlab-wiki-home/-/wikis/Operations/Servers/BML-Server-Notes)

*  [BMLabServer](#bmlabserver)
*  [CentOS操作手記](#centos操作手記)
*  [GIBMS_Server教學文件](#gibms_server教學文件)-所上網站、實驗室網站資訊
*  [新增使用者與系統重灌](#新增使用者與系統重灌)
*  [Server 開關機教學](#server開關機教學)
*  [BMLabServer ID 清單](#伺服器id清單)-尚待編輯
*  [BMLabServer NAS](#nas)
*  [Server Config](#server-config)
*  [Use Anaconda on server](#use-anaconda-on-server)
*  [BMLab Server 架設網站](#bmlab-server-架設網站)
   * [Apache](#apache)
   * [Firewall 設定](#firewall-設定)
   * [Server 網站架設](#server-網站架設)
   * [Server 網站設定](#server-網站設定)
   * [UserDir 設定](#userdir-設定)


## BMLabServer ##

完整請參考：[CentOS操作手記](#centos操作手記)

參考：[150415 - BMLabServer](https://www.evernote.com/shard/s604/sh/8627faa4-39e8-4bfc-afeb-ac184ab7266b/c1c9cda91887ea0eca2fc91e7cbc776b)

*  Current server ip info
   *  BMLabUnit00: 140.112.123.123 (location: 1554)
   *  BMLabUnit01: 140.112.123.8 (location: 1550)
   *  BMLabUnit02: 140.112.123.124 (location: 1550)
   *  BMLabUnit03: 140.112.121.218 (location: 4F, unlimited data)

*  Firewall setting (BMLabUnit00-02)
   *  To avoid the hacker attack from Europe or some where else, we set up the firewall for the domain only from NTU can be ssh to our server (In /etc/hosts.allow 140.112.0.0/16 & 192.168.123.0/24, and /etc/host.deny ALL). Therefore, everyone need to login as NTU domain to access our server.
   *  [How to set up VPN as NTU domain](https://ccnet.ntu.edu.tw/vpn/)

*  RAID: 10.0.0.2
   *  administrator/password
*  Switch: 10.0.0.6
* [Install PHP 7.2](https://www.tecmint.com/install-php-7-in-centos-7/)
*  [Install phpMyAdmin](https://www.hostinger.com/tutorials/how-to-install-phpmyadmin-on-centos-7/):
   * root/BMLab-Goh5993
* Install MySQL 5.7.26

*  Dropbox：
   * 程式 @ `/root/.dropbox-dist/`
   * 檔案 @ `/usr/local/bin/Dropbox/`
   * 網站 Backup：建立軟連結 `ln -s /var/www/html/ `
      * 內網：
         * 網頁：`/usr/local/bin/Dropbox/BMLab_Backup/BMLab_Apache/`
         * SQL：`/usr/local/bin/Dropbox/BMLab_Backup/BMLab_SQL/`
   * 排程：每天一點到四點依序更新
      * Code: `/usr/local/bin/Dropbox/BMLab_Code/ServerBackup/`
      * 0100 ~ 0200 : WebFile
      * 0200 ~ 0300 : Shiny
      * 0300 ~ 0400 : Wordpress
      * 0400 ~ 0500 : Dropbox
*  GitHub : /usr/local/bin/Dropbox/BMLab_Code/
   * ntubmlab@gmail.com/BMLabGoh5993
*  刪除暫存檔：/tmp/
   * 列出一個月之前的檔案：ls -lrt `find /tmp/ -mtime +30`
   * 刪除一個月之前的檔案：\rm -fR `find /tmp/ -mtime +30`
*  `find / -nouser -nogroup`


## CentOS操作手記 ##
參考：[CentOS操作手記](https://www.evernote.com/shard/s604/sh/a33193f9-e85b-475b-98f8-3db05176df9d/dca35148f19893129aa25131264a16fe)

### 前言
本篇是希望將自己目前分開編寫、CentOS 維護時所需的知識跟經驗彙整在一起所做得重新整理，各伺服器特別的設定，尤其是帳密方面，請參閱精簡後的原文： [150415 - BMLabServer](#bmlabserver)、 [150423 - GIBMSServer](#gibms_server教學文件)

### 系統
*  CentOS 7
*  擴充套件：EPEL 增益集（企業版 Linux 附加軟件包）
*  安裝：依 CentOS 版本編號決定，如 CentOS 6 應為 epel-release-lastest-6.noarch.rpm

http://www.nic.funet.fi/pub/mirrors/fedora.redhat.com/pub/epel/

(su) rpm -Uvh #rpm_weblink#

### 系統節電
閒置時間：系統 => 偏好設定 => 螢幕保護程式 => 在此時間之後將電腦設為閒置

### 帳號管理
### GUI
*  命令提示行（command-line interface, CLI / 終端機）
*  設定：終端機 => 編輯 => 設定組合偏好設定
*  推薦介面配色為深藍底鵝黃字：一般 => 使用系統的固定寬度字型 => font size 18
*  顏色 => 使用系統佈景主題指定的色彩 => 內建色彩組合：自訂 => 文字顏色：#FCFDCA，背景顏色：#1C1647
*  檔案瀏覽器（File Browser）
*  設定：在任何檔案視窗 => Edit
*  推薦開新資料夾不開新視窗
*  Behavior => Always open in browser windows
### 系統服務
*  PHP 支援：PHP
*  設定檔：php.ini
*  /etc/php.ini
*  網路瀏覽器：Firefox
*  網路瀏覽器：Chrome（BMLab only） <= 想刪乾淨
*  統計軟體：R
*  目前版本：R 3.1.3 #150423#
*  安裝：需先安裝 EPEL 增益集
`(su) yum install R`

### 套件
開發者套件（可以安裝 CRAN 以外的套件組）：devtools
*  安裝：
*  install.packages("devtools"); library(devtools);
*  支援動態文件：rmarkdown
*  安裝：需要 devtools
https://groups.google.com/forum/#!topic/shiny-discuss/5t_x6vAM45Y

`install_github('rstudio/rmarkdown')`



## GIBMS_Server教學文件 ##
參考：[GIBMSServer 教學文件](GIBMSServer 教學文件)
**Linux**
*  CentOS 6.6 (150423Final)
   *  hostname: localhost.GIBMSWeb
   *  IP: 140.112.122.160
*  帳戶：
   *  root/GIBMSBM153988920
   *  gibmsadmin/GIBMSBM153988920
   *  hshextwebadmin/a8j5iud098h1
   *  拜託請保留 skylikewater (JhengTingChen)
*  FTP upload in Wordpress: 選 SSH2，然後 Private Key 為空
* 用FileZilla上傳/下載文件：
  - 主機：140.112.122.160
  - 協定：SFTP (FTP會進不去)
### 網路服務

*  網頁：Apache 網頁資料夾：`/var/www/html/`
*  設定檔：httpd.conf
*  /etc/httpd/conf/httpd.conf
*  設定網域 => "ServerName #ServerIP#:80"

### MySQL 管理

*  phpMyAdmin 資料夾：`/var/www/html/ThisIsNotphpMyAdmin/`
*  網址：http://#ServerDNS#/ThisIsNotphpMyAdmin/
*  安裝： 要注意目前新版的 Apache 已經內含 phpMyAdmin
*  phpMyAdmin 2.11.11.3：http://gibms.mc.ntu.edu.tw/ThisIsNotphpMyAdmin/
*  phpMyAdmin 4.4.15.6: http://gibms.mc.ntu.edu.tw/phpmyadmin
   *  root/BMLabGoh5993
   * phpMyAdmin 4.4.15.6 的網頁設定檔在`/etc/httpd/conf.d/phpMyAdmin.conf`，若是遇到permission denied可以進去設定
### 網站   
*  所上網站 (by Wordpress)：http://gibms.mc.ntu.edu.tw/
   *  後臺：http://gibms.mc.ntu.edu.tw/wp-admin
   *  GIBMSAdmin/GIBMSBM153988920
   *  150428 - Wordpress 升 4.2.1
   *  新增作者帳戶 GIBMSWriter/GIBMSBM153988920
*  英文網站 (by Wordpress) ：http://gibms.mc.ntu.edu.tw/Eng/
   *  後臺：http://gibms.mc.ntu.edu.tw/wp-admin
   *  GIBMSAdmin/GIBMSBM153988920
   *  150429 - Wordpress 升 4.2.1
   *  新增作者帳戶 GIBMSWriter/GIBMSBM153988920
*  各實驗室網站
   *  吳恩賜老師 BMLab (by Wordpress)：http://gibms.mc.ntu.edu.tw/bmlab/
   *  後臺：http://gibms.mc.ntu.edu.tw/bmlab/wp-admin/
   *  root/BMLabGoh5993
   *  BMLabWeb/BMLabGoh5993
*  Shiny：http://gibms.mc.ntu.edu.tw:3838/

## 新增使用者與系統重灌 ##

* 使用時機
  - 有新成員加入
  - 在系統重灌

* 新增使用者 [(詳見該網站)](https://www.lifewire.com/create-users-useradd-command-3572157)  

```
# 先登入root帳號
su
# 增加使用者(名為coolperson)
useradd -m coolperson

# 設定coolperson的密碼
sudo passwd coolperson

cat /etc/passwd
# 檢查是否成功：查看目前所有的users
```  


* 如果系統重灌，則需要把舊的檔案更改所有人權限 [(詳見該網站)](https://www.hostingadvice.com/how-to/change-file-ownershipgroups-linux/)

```
# 把directory這個資料夾的所有人更改為coolperson
chown -R coolperson directory/
```
* 如果系統重灌，透過X2GO登入會有SSH key認證失敗的問題。
  - 解決方法：
    - 到dropbox實驗室BML的google帳號登入，下載"BMLabServer_DSAKey.txt"這個檔案。
    - 到X2G的Session preference 設定成 "XFCE"
    - 在RSA/DSA KEY的欄位，使用下載到的"BMLabServer_DSAKey.txt"
    - 重新登入即可

* 登入後重新設定密碼
```
passwd
```
## NAS ##
Ver. 160123 (參考：[BMLabServer - NAS](https://www.evernote.com/shard/s604/sh/9cdb536c-2031-4c7f-a9f6-4f3aa53a8ed9/f34a6d71de11ac044941488e7a40ddf4))

*   IP: http://140.112.121.217/
*   型號：EMS5 NAS 3000-SRL16
   *   容量：32 TB（實際約 29.104 TB）
*   RAID 實體磁碟：
   *   16 個 1.819 TB SAS 磁碟，其中 port 14-15 系統保留用。
*   邏輯磁碟分割：
   *  sysvol：系統用碟   
      *  2.38 GB
   *  BMLabISCSI：iSCSI  
      *  1.95 TB
      *  試用的保留空間
   *  BMLab：Share 碟   
      *  19.87 TB
      *  開 CIFS, FTP, NFS, WebHD <= 網頁版登入看到的是這個硬碟
*  資料夾：都在 BMLab 邏輯磁碟下
   *  home：設定為目前的家目錄
   *  BMLab：預計 rsync 過去的資料夾
*  預計管理策略：
純全機備份 BMLabServer 的 RAID（也就是 `/bml/Data/` 全部而已）
*  未來方向
   *  納入資料庫備份（MySQL 的實體檔案在 `/var/lib/mysql` 下，而目前 dropbox 備份仍不夠自動）
   *  開設實驗室內部用 FTP

## 伺服器id清單 ##

Version-201903: [ID list](https://docs.google.com/spreadsheets/d/1lYCeZGWOa-AZC9WJ4MOwLE4pMkQ83MurMpQRhEj5pK0/edit#gid=337649233)

參考：[Evernotes: BMLabServer - ID 核對](https://www.evernote.com/shard/s604/sh/ce0bcc0b-31be-4ae8-903f-de8ae13e5d8f/5f279ae5a28b189674f8f688e4f56cb3)

## Server Config ##
[Server Config](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/server_config.md)

## Use Anaconda on server ##
[Use Anaconda on server](https://gitlab.com/brain-and-mind-lab/notes-for-bml/notes-for-analysis/blob/master/server_anaconda_navigator.md)

#### Install Packages
You may want to install several packages under the Anaconda:

1. pandas: `conda install -c anaconda pandas `
2. matplotlib: `conda install -c conda-forge matplotlib`

#### Tensorflow
See [Tensorflow](https://gitlab.com/brain-and-mind-lab/notes-for-bml/bmlab-wiki-home/wikis/teach#tensorflow)

## BMLab Server 架設網站

#### Apache
1. 確認server是否有安裝Apache:
    * 一開始要先確認server是否有安裝Apache，可以用`locate apache` 找
    * 若是 server 沒有安裝Apache，則需要安裝: `yum -y install apache2 `
2. 確認server是否有安裝httpd: 可以用`locate httpd` 找
    * 若是沒有安裝，則需要安裝: `yum -y install httpd httpd-tools`

3. activate httpd service: `systemctl enable httpd.service`
    * 若是command not found: ` yum install systemd`
    * 參考[鳥哥](#http://linux.vbird.org/linux_basic/0560daemons.php#systemctl_cmd)
    * 或是可以用`service`這一個command： `service httpd enable`; `service httpd restart`;  `service httpd status `

4. list enable services: `systemctl list-unit-files --state=enable`
5. check httpd services: `systemctl status httpd`





#### Firewall 設定
1. `iptables -L` 看firewall資訊:
  * Chain INPUT (policy ACCEPT): 誰進來
  * Chain FORWARD (policy ACCEPT)：經過我們
  * Chain OUTPUT (policy ACCEPT)：我們送出

2. 打開httpd: `firewall-cmd --permanent --add-service=http`
3. Reload FirewallD so the rules take effect immediately:  ` firewall-cmd --reload`


{- 防火牆設定需要非常小心！！若是沒設好，我們server的資料很容易被大家看光光。 若不需要網站，可以關掉Apache(`systemctl stop httpd`)，以防被駭-}
#### Server 網站架設

1.  `/var/www/html` 為DocumentRoot:

    The directory out of which you will serve your documents. By default, all requests are taken from this directory, but symbolic links and aliases may be used to point to other locations. configuration, error, and log files are kept.

    {+ p.s. 這個default的名稱設定在: cd /etc/httpd/conf/httpd.conf 裡面 +}

2. 目前default顯示的網頁都是: index.html，這個default的名稱設定在：`cd /etc/httpd/conf/httpd.conf` 裡面
3. 目前140.112.121.218的`/var/www/html` 底下的index.html內容如下：
```
<!DOCTYPE html>
<html>
<head>
<title>BMlab</title>
</head>
<body>
<h1>Hi, we are NTU BMlab!</h1>
</body>
</html>

```
4. 啟動Apache: `systemctl start httpd`
5. 在網址列輸入server IP（140.112.121.218），就會出現  `/var/www/html/index.html `的內容了！
#### Server 網站設定
 設定Apache configuration的檔案在：`cd /etc/httpd/conf/httpd.conf`， 裡面包含：



*  ServerRoot
* Listen
* User/Group
* ServerAdmin
* ServerName
* DocumentRoot

 等等的設定


 {+ 為了確保server安全，目前140.112.121.218有IP限制 +}

 目前的設定：
 ```
 <Directory "/var/www">
     AllowOverride None
     # Allow open access:
     Require all denied
 </Directory>

 # Further relax access to the default document root:
 <Directory "/var/www/html">
     #
     # Possible values for the Options directive are "None", "All",
     # or any combination of:
     #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
     #
     # Note that "MultiViews" must be named *explicitly* --- "Options All"
     # doesn't give it to you.
     #
     # The Options directive is both complicated and important.  Please see
     # http://httpd.apache.org/docs/2.4/mod/core.html#options
     # for more information.
     #
     Options Indexes FollowSymLinks

     #
     # AllowOverride controls what directives may be placed in .htaccess files.
     # It can be "All", "None", or any combination of the keywords:
     #   Options FileInfo AuthConfig Limit
     #
     AllowOverride None

     #
     # Controls who can get stuff from this server.
     #
     Require all denied
 </Directory>

 ```
 若是你要打開，讓140.112.121.218有東西，請改成：
 ```
 <Directory "/var/www">
     AllowOverride None
     # Allow open access:
     Require all granted
 </Directory>

 # Further relax access to the default document root:
 <Directory "/var/www/html">
     #
     # Possible values for the Options directive are "None", "All",
     # or any combination of:
     #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
     #
     # Note that "MultiViews" must be named *explicitly* --- "Options All"
     # doesn't give it to you.
     #
     # The Options directive is both complicated and important.  Please see
     # http://httpd.apache.org/docs/2.4/mod/core.html#options
     # for more information.
     #
     Options Indexes FollowSymLinks

     #
     # AllowOverride controls what directives may be placed in .htaccess files.
     # It can be "All", "None", or any combination of the keywords:
     #   Options FileInfo AuthConfig Limit
     #
     AllowOverride None

     #
     # Controls who can get stuff from this server.
     #
     Require all granted
 </Directory>

 ```
#### UserDir 設定

上面已經為我們server 架設好網站：http://140.112.121.218，若是你想要在異己的home下面有自己的網站，就需要 UserDir設定，如此一來http://140.112.121.218/~user 就可以到你的網站了

例如：http://140.112.121.218/~hsinyi，就可以到 `/home/hsinyi/public_html/index.html`的網站

1. 先到你自己的 /home/user/下面創建一個資料夾：public_html

    {- 注意！  ~userid must have permissions of 711, ~userid/public_html must have permissions of 755, and documents contained therein must be world-readable. Otherwise, the client will only receive a "403 Forbidden" message.-}


    也就是說，你創建完public_html要確認權限問題，用[chmod](http://linux.vbird.org/linux_basic/0210filepermission.php#chmod) 改你的資料夾權限
2. 到 `/etc/httpd/conf.d/userdir.conf`改設定：

    * Comment 掉 UserDir disabled
    * Uncomment UserDir public_html
3. restart Apache: `systemctl restart httpd`
4. 試著在網址列輸入你的路徑看看可不可行，若是forbidden，可以試試： `chcon -R --type=httpd_sys_rw_content_t webpath/` 其中，webpath/是你的路徑，例如`chcon -R --type=httpd_sys_rw_content_t hsinyi/`
