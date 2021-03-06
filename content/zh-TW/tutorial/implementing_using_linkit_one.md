# LinkIt ONE 實際操作

下面是一個簡單的指南來快速設置您的LinkIt One 開發板（Aster 2502 ）連接到MediaTek Cloud Sandbox。

## 情境

此教程的情境為，替您的LinkIt One開發板建立一個測試裝置，替您的板子上傳一段Arduino code，且使用RESTful API來使您的開發板能上傳位於D13的LED燈光狀態至MCS，同時也可以使用MCS透過TCP Socket來遠程控制LED狀態。


![](../images/LinkIt-one-tutorial/img_scenario.png)


## 設置準備

為了要完成此設置，您必須先：

1. 您的開發板必須有電源連接，電源可來自電池或是micro-USB。
2. 您的開發板必須有Wifi連結。


此外，您不需要額外的電子組件，就可以連接到開發板了。


### 步驟一　替您的產品原型新增一個開關類型的資料通道

a. 登入平台後，點選上方的"開發"，並且點擊新增按鈕，來新增一個新的產品原型。

![](../images/screenshot/screen_shot-01.jpg)

b. 請跟著畫面只是填入產品原型的基本資訊：

![](../images/screenshot/screen_shot-08.jpg)

c. 點擊您剛建立的產品原型下方的"詳情"按鈕

![](../images/screenshot/screen_shot-09.jpg)

d. 在產品原型詳情頁面中，點擊"資料通道"分頁，並且點擊新增按鈕來新增一個資料通道：

![](../images/screenshot/screen_shot-10.jpg)

在此教程中，我們需要建立兩格資料通道，一個是顯示器類型的，用來表示開發板上LED燈光的狀態；另一個則為控制類型的，用來對您的開發板LED燈光狀態下指令:


e. 點擊在"顯示器"下方的新增按鈕，並輸入已下資訊：

![](../images/screenshot/screen_shot-11.jpg)

![](../images/screenshot/screen_shot-12.jpg)

請注意，您在此輸入的資料通道Id是獨特不可重複的，之後將會在呼叫API時使用到。

f. 重複步驟e，但是把顯示器改成"控制器"，並且輸入以下資訊：

![](../images/screenshot/screen_shot-13.jpg)

![](../images/screenshot/screen_shot-14.jpg)

請注意，您在此輸入的資料通道Id是獨特不可重複的，之後將會在呼叫API時使用到。

g. 完成後，您將會有以下的資料通道：

![](../images/screenshot/screen_shot-15.jpg)

### 步驟二　建立測試裝置

a. 點擊畫面右上方的"創建測試裝置"

![](../images/screenshot/screen_shot-16.jpg)

b. 輸入測試裝置名稱和描述：

![](../images/screenshot/screen_shot-17.jpg)

c. 測試裝置建立後，點擊"詳細資訊"連結至測試裝置詳情頁面:

![](../images/screenshot/screen_shot-18.jpg)


![](../images/screenshot/screen_shot-19.jpg)

請注意，deviceId和deviceKey是獨特不可重複的，之後將會在呼叫API時使用到。

### 步驟三　取得DeviceId，DeviceKey，以及資料通道ID

下方的摘要訊息為我們與測試裝置溝通時所需要的必要欄位：

| 名稱 | 值 | 備註 |
| -- | -- | -- |
| deviceId | Dsre1qRQ | 測試裝置的獨特識別碼 |
| deviceKey | DFbtsNWg4AuLZ30v  | 測試裝置的API Key |
| dataChannelId | LED | 此教程中所需顯示器類型LED燈號的資料通道識別碼 |
| dataChannelId | LED_CONTROL | 此教程中所需控制器類型LED燈號的資料通道識別碼 |

請注意，此教程中圖片所顯示的deviceId 和deviceKey會和您實際操作時取得的不一樣，請使用您所取得的值。


### 步驟四　替您的開發板編程

操作流程如下：


a. 呼叫RESTful API：
GET api.mediatek.com/mcs/v2/devices/{deviceId}/connections.csv
來取得Socket Server IP 和連接阜的值。

b. 和Socket server建立TCP connection

c. 呼叫RESTful API:
POST api.mediatek.com/mcs/v2/devices/{deviceId}/datapoints.csv
每五秒上傳一次位於開發板D13位置的LED燈狀態

d. 並且保持TCP connection連線隨時可以接收來自MCS的指令

e. 每90秒更新一次TCP connection的heartbeat連線

請點擊此連結來下載Arduino範例程式 [here](https://raw.githubusercontent.com/Mediatek-Cloud/MCS/master/source_code/linkit_sample_ino.ino)

請注意:
使用此範例程式需要HttpClient，您可使用此連結下載
[here](https://github.com/amcewen/HttpClient/releases)

### 步驟五　讓您的開發板開始動作吧！

當您將範例程式上傳至開發板後，請確保開發板有Wifi網路連線，並且有定時傳送訊號顯示裝置已連接並正在待命中：

![](../images/LinkIt-one-tutorial/13-Test-Device.JPG)

您現在可以去裝置詳情頁面，並看到您剛建立的LED控制器，當您將控制器狀態設成開，開發板上的LED燈即會打開，同時，您將可以看到您所建立的LED顯示器的狀態也改變為開了。反之亦然，若控制技設為關，燈即會關閉，並顯示器狀態改為關。


![](../images/screenshot/screen_shot-20.jpg)

![](../images/screenshot/screen_shot-21.jpg)

![](../images/LinkIt-one-tutorial/16-Test-Device.JPG)

恭喜！您已完成此教程！








