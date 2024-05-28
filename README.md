
# VMware虛擬化環境進行簡易的iSCSI配置-NAS以QNAP為例

## 目錄
---
- 環境說明
- VM設定
- NAS設定
- 建置結果展示以及驗證
---

## 一、環境說明

本文章僅為了讓使用者可以順利使用iSCSI進行NAS儲存空間與Server進行連接而撰寫，會簡單提點iSCSI需要的設定與步驟。

:bulb:開始前先介紹我會使用的設備：VM主機、NAS、網路線*2、Switch（L3 Firewall）。




可以簡單參考下圖的配置。


![image](https://hackmd.io/_uploads/Sk9CDuVXA.png)


### （一）、NAS環境檢視

說明NAS, 請參考下圖：
![PXL_20240517_062105120.MP](https://hackmd.io/_uploads/BJgGtOVXR.jpg)

![PXL_20240517_062122680.MP](https://hackmd.io/_uploads/SJIMY_4XA.jpg)

![image](https://hackmd.io/_uploads/SJQPqtEX0.png)


![image](https://hackmd.io/_uploads/SJtR5_NXA.png)




說明：第一張圖是NAS後面的網路插口，對應到第四張圖所框選之處。第二張圖是網路線所走的最後位置，是為Firewall（L3）。



### （二）、Server環境檢視

為使用iSCSI, Server需要貢獻一個Port口或者說一張網卡，進行`VMkernel 連接埠`的設定。參考如下：


![PXL_20240517_064238857.MP](https://hackmd.io/_uploads/Syp3hu4QR.jpg)

![PXL_20240517_064300053.MP](https://hackmd.io/_uploads/SJjpn_NQA.jpg)

![image](https://hackmd.io/_uploads/HJsC2OV7C.png)


至此，基本環境檢視結束，進入VM設定。


## 二、VM設定


首先先將剛剛對VM需要使用的網路卡進行設定，使其成為`vSwitch` 還有設定`連接埠群組`詳細虛擬化環境網路建置的部分可以去看我先前撰寫的 [文章](https://hackmd.io/@JANALEXEI88/BybM5slm0)。


![image](https://hackmd.io/_uploads/SJDWJF47R.png)

![image](https://hackmd.io/_uploads/SkxaJtNQA.png)


這裡比較重要，新增一個`VMkernel NIC`，最後在`儲存區` `介面卡`新增一個供iSCSI功能的設定，使用`軟體iSCSI`將剛剛的`VMKernel`加入。

:bulb:這裡其實要設定介面卡，新增並且啟用`iSCSI`。

💡 更新：這裡`VMkernel NIC`不要設定和NAS端口相同的IP，例如`VMkernel NIC`設定IP.166，NAS的Port IP.166，會衝突。

請將`VMkernel NIC`假設另一組IP, 例如.200。

![image](https://hackmd.io/_uploads/H1SXeFE7R.png)

![image](https://hackmd.io/_uploads/Hk3sgFE7A.png)

VMWare端設定結束。



## 三、NAS設定

先將剛剛在環境檢視看到的那個Port進行固定IP設定。

新增iSCSI目標，並切`LUN`給他，確認`網路入口`設定正確。


![image](https://hackmd.io/_uploads/SkHizK4m0.png)

![image](https://hackmd.io/_uploads/S16FEY4mC.png)



NAS端設定完成，使用虛擬機進行iSCSI連接測試。




## 四、建置結果展示以及驗證

驗證結果，直接輸入剛剛設定好NAS Port的`固定IP`, 我這邊設定為：`10.100.100.166`，直接數入就可以看到我切的LUN了。



![image](https://hackmd.io/_uploads/ryPjIKVmA.png)




各環境下的設定不同，文件僅供參考。

