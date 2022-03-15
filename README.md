# 主題三：Camera Calibration（相機校正）

:::info
筆記標記說明：
🔴 代表透過 Putty 使用 SSH 連線到樹莓派上
🔵 代表在 VM 上操作
:::

**請記得！執行任何 ROS 的程式都要先進入 ROS 環境，也就是開啟 ROS master。**

DuckieBot 為什麼需要校正？
因為 DuckieBot 是使用魚眼鏡頭所以會有扭曲失真的問題。

為什要用魚眼鏡頭？
因為魚眼的可視角度較廣。

相機校正需要找到兩個參數矩陣：內參矩陣、外參矩陣。

## 🔴 相機測試
校正前要先確認相機可不可以用！

```shell=
cd ~/duckietown
source environment.sh
roslaunch duckietown camera.launch veh:=duckpi1
```
請將 line3 的 duckpi1 改成自己的 duckiebot's name

執行後會有此畫面
![](https://i.imgur.com/VA7fDbY.png)

## 🔵 執行 Rviz
```shell=
cd ~/duckietown
source environment.sh
source set_ros_master.sh duckpi1
rviz
```
請將 line3 的 duckpi1 改成自己的 duckiebot's name

![](https://i.imgur.com/bZGS1z6.png)

![](https://i.imgur.com/3XFCGGN.png)

:::info
如果無法在Rviz看到影像畫面請舉手找助教。或是在樹莓派試試看下列指令：
```shell=
raspistill -v -o test.jpg
find | grep " test.jpg "
```
如果可以找到 test.jpg 代表樹莓派有拍照並存檔，看看虛擬機那邊有沒有出錯。
:::

## Intrinsic Calibration
### 🔴 執行相機程式
```shell=
cd ~/duckietown
source environment.sh
roslaunch duckietown camera.launch veh:=duckpi1 raw:=true
```
請將 line3 的 duckpi1 改成自己的 duckiebot's name

### 🔵 執行相機內部校正程式
```shell=
cd ~/duckietown
source environment.sh 
source set_ros_master.sh duckpi1
roslaunch duckietown intrinsic_calibration.launch veh:=duckpi1
```
請將 line3、line4 的 duckpi1 改成自己的 duckiebot's name

![](https://i.imgur.com/a6pOM7V.png)

![](https://i.imgur.com/282Jjab.png)


### 🔴 內參數校正相機完成

按下 Commit 後樹莓派會顯示儲存校正檔的訊息

![](https://i.imgur.com/RzqhtDF.png)

## Extrinsics Calibration

### 將小鴨車輪子與邊線對齊
![](https://i.imgur.com/1NbX7F9.png)


### 🔴 執行相機外參數校正

先按 ctrl+c 停掉剛剛的內參數校正功能，再執行外參數校正
```shell=
cd ~/duckietown 
source environment.sh
rosrun complete_image_pipeline calibrate_extrinsics
```
![](https://i.imgur.com/G1A61Wd.png)

如果出現下方的錯誤，請回到投影片 p5 跟 p7，調整相機鏡頭的焦距，直到沒有失焦為止。
![](https://i.imgur.com/locWIRE.png)

```shell=
cd ~/duckietown/out-calibrate-extrinsics
sudo apt install eog
sudo eog all.jpg 
```


###### tags: `ROS`
