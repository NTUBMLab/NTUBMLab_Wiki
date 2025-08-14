
## 目錄 ##
*  [AI Robohon 機器人設定](#ai-robohon-機器人設定)
*  [AI Robohon APP 重裝](#ai-robohon-app-reinstall)



## AI Robohon 機器人設定


#### Wifi連線
1. 小電腦(pwd: stupidRobohon)和RoBoHoN開機
2. Sapido插電後亮燈，調為W
3. 小電腦連wifi,選擇「Sapido_BRF61c_2.4g_d8e0aa」,若顯示「沒有網際網路,開放」代表連線成功
4. 打開終端機，輸入 `ipconfig`，確認IP是`192.168.1.100`
5. RoBoHoN連wifi

   A. 選擇黃色小齒輪的設定按鈕,連接wifi

   B. 選擇「Sapido_BRF61c_2.4g_d8e0aa」

   C. {-注意!步驟2和3的順序不能搞混-},一定要先連小電腦,才能連RoBoHoN的,檢查方式:
   * 在RoBoHoN連上wifi之後,再按一下「Sapido_BRF61c_2.4g_d8e0aa」按鈕,檢查彈出視窗中的「IP address」是否為「192.168.1.101」。
   * 若最後的數字部分為100,代表次序搞混,請斷開小電腦和RoBoHoN的wifi,重新按照次序連線

#### 電腦設定

1. 在工作列點選「命令提示字元」(圖示為黑色方塊的)
2. 輸入 `cd Desktop\test_robohon\server`
3. 輸入 `python server.py`
4. 點選「windows開始」-> 「XAMPP」-> 「XAMPP Control Panel」
5. 「module Apache(第一行)」 後面點選start -> 點選Admin,會直接跳轉至網頁

#### 機器人設定
1. 等到上述步驟全部完成後,開啟RoBoHoN的App(貓貓圖案)
2. 回到「命令提示字元」,若有不斷自動跳出empty/None,代表設置成功
3. 若是遇到機器人當掉，可以嘗試：設定 > 其他 > 應用 > 刪掉 RemoteControl 這一個APP

## AI Robohon APP reinstall

本文件說明在AI Robohon project 中，要將Robohon APP 重新安裝的指南

1. 下載 Android Studio
2. 下載 [Robohon template](https://drive.google.com/file/d/1sxalF1i5h6B44UxL-LTSVF8KUVbtA2eV/view?fbclid=IwAR1Y9GMtW69yhTOP-083yUIr85xg-eBAWcSdLr6XDjCgepZtDrYFn2DIZf0)
3. 下載 [Robohon APP](https://developer.android.com/studio/?gclid=CjwKCAjwqLblBRBYEiwAV3pCJnTMLSZcpmMsa6PvSdInsqcnRWSmNup_3GxXcI1IxFE2takh73LJqRoClOEQAvD_BwE#downloads)

4. 將下載的 `/AndroidStudioProjects/RoBoHoN_Remote_s/RoBoHoN_Remote/Robohon_web/android_app/RemoteControl` 複製到自己的 `/AndroidStudioProjects/` 路徑下
5. open exsiting project > 選則 Remote Control
6. 試著按下 Run，看會有什麼訊息
7. 若出現以下的 {-Error: Fail to find target with hash string 'android-21'-}:
   ![Robohon_APP_error_1](uploads/Robohon/Robohon_APP_error_1.png)
    * 點選 Tool > Android > SDK manager > 勾選 API level 21 > 點選 Apply 進去安裝 > OK

![Robohon_APP_error_7](uploads/9c1313760bfefb65b7705221d8a50202/Robohon_APP_error_7.png)
![Robohon_APP_error_9](uploads/cee340b55fc80c911c24ae1bfacd1954/Robohon_APP_error_9.png)
8. 若出現以下的 {-Error: Fail to find Build Tools revision 27.0.3-}:
   ![Robohon_APP_error_3](uploads/Robohon/Robohon_APP_error_3.png)
    * 點選 Tool > Android > SDK manager > 勾選 SDK Tools > 點選 Show Package Detail > 點選  27.0.3 > Apply 進去安裝 > OK
    ![Robohon_APP_error_5](uploads/Robohon/Robohon_APP_error_5.png)
9. 連接 Robohon 機器人 ， 按下 Run，及重新開始安裝機器人

    * 她會出現一個 warning message，問你要不要覆蓋原本的app，請按OK!
