# PYNQ-Z2-note


## 連線設定

1. 用 USB 線連接到電腦(電源)
2. 用網路線將板子跟電腦的網路孔對接
3. 指定筆電乙太網路卡靜態IPv4:192.168.2.X，X可以是除了99以外0~254的數字
    
![image](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/686281d0-2948-4d8e-b035-179b4cb7306b)

4. 在瀏覽器輸入 [http://192.168.2.99](http://192.168.2.99/) 即可連到jupyter的web介面
5. 如果配置正確，就會看到登入畫面。使用者名稱是 xilinx，密碼也是 xilinx


## 透過samba傳輸文件
非必要，因為在 jupyter 上也可以上傳下載文件

1.在我的電腦，輸入 \\pynq ，Enter，輸入網路憑證：使用者名稱和密碼都是 **xilinx** 

2.將出現以下畫面，點進去的 pynq\xilinx\jupyter_notebooks 就是之前我們在jupyter中看到的文件目錄了

![image](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/a73d0e67-4a1b-4dc0-9587-c63854fea6b2)

## 如何加入PYNQ-Z2 board files到Vivado

1.先下載pynq-z2板文件

https://github.com/xupsh/pynq-supported-board-file?tab=readme-ov-file

2.下載後將文件複製到  C:\Xilinx\Vivado\2018.2\data\boards\board_files

(2024.1 version :C:\Users\ander\AppData\Roaming\Xilinx\Vivado\2024.1\xhub)

3.重開vivado，完成


## 範例程式

### LED

按下按鈕 0：RGB LED 燈變色

按下按鈕 1：LED 燈由右至左移動 (LD0 -> LD3)

按下按鈕 2：LED 燈由左至右移動 (LD3 -> LD0)

按下按鈕 3：關閉所有 LED 燈並結束此示範


```
from time import sleep
from pynq.overlays.base import BaseOverlay

base = BaseOverlay("base.bit")

# 按鈕觸發後的反應時間
Delay1 = 0.3
Delay2 = 0.4
# RGB LED的顏色
color = 0
# 指定哪些RGB LED（在這裡是位置4和5）被控制
rgbled_position = [4,5]

for led in base.leds:
    led.on()    
while (base.buttons[3].read()==0):
    if (base.buttons[0].read()==1):
        color = (color+1) % 8
        for led in rgbled_position:
            base.rgbleds[led].write(color)
            base.rgbleds[led].write(color)
        sleep(Delay1)
        
    elif (base.buttons[1].read()==1):
        for led in base.leds:
            led.off()
        sleep(Delay2)
        for led in base.leds:
            led.toggle()
            sleep(Delay2)
            
    elif (base.buttons[2].read()==1):
        for led in reversed(base.leds):
            led.off()
        sleep(Delay2)
        for led in reversed(base.leds):
            led.toggle()
            sleep(Delay2)                  
    
print('End of this demo ...')
for led in base.leds:
    led.off()
for led in rgbled_position:
    base.rgbleds[led].off()
```
