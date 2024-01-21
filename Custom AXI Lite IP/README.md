# Creating a custom AXI Lite IP in Vivado 

### 透過 PYNQ 來控制 FPGA 上的自定義 IP 來操作 LED 燈

## 1.開一個新專案，板子選擇PYNQ-Z2。若找不到，參考前一頁的方法加入板子

![image](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/95824b87-74f8-48ab-b70e-122d393c5d89)

## 2. 建立 AXI4 Peripheral
### AXI4 Peripheral:
* 在 FPGA 設計中常見的一個步驟，特別是在使用基於 ARM 的系統，如 Xilinx 的 Zynq 系列 SoC（System on Chip）

* AXI4（Advanced eXtensible Interface 4th generation）是 ARM 公司開發的一種高效能內部連接標準，用於模塊之間的數據傳輸

* 通常指的是一個自定義的模塊，它透過 AXI4 接口與處理器（如 ARM 處理器）進行通訊。可以是各種不同的硬體功能，比如自定義計算單元、數據處理塊、或者特定的 I/O 控制器等

![未命名](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/5b9c4293-63fa-4f0d-b4f2-2954a0897b52)

![未命名1](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/411e4f4f-0956-40f6-92b2-f89d48fda3bc)

![未命名2](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/200525b2-6614-4093-88dc-414c95c1dc9a)

![未命名3](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/977c7dee-d680-4f30-aa1d-9611f8db1591)

## 3.分別修改內建程式碼，加入自訂的 port

![4](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/a1246920-3c0d-4559-8484-c5492209182b)

![5](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/e7cad5d9-8ceb-4076-bfdf-728695a87b11)

## 4. Run Synthesis 和 Run Implementation，完成後再 re-package IP

![6](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/5ca7236c-fc56-4a4b-936b-78b90a8ba0c8)


## 5.回到block design，將剛剛包好的 IP 加入，再點自動連接 (記得將LED 的接口拉出來)

![7](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/5eaae914-7a39-40be-8a4b-a41bfbf05ceb)

![8](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/24265999-9dea-4a89-bd4e-780ad7510756)

## 6.加入PYNQ Z2 的 constraint file，

### Source : https://github.com/Xilinx/PYNQ/blob/master/boards/Pynq-Z2/base/vivado/constraints/base.xdc

![9](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/49de1e0a-a42b-488a-aac8-bc9cdd986f93)

## 7. 改LED 接口的名稱，要跟 constraint file 裡面的名稱一樣
![10](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/eeeb2497-4e0e-4f14-88e8-84a664fc59ea)

## 8. "Create HDL Wrapper" ，包裝 Block Design 檔案。這個步驟的主要目的是為了建立一個頂層文件

![11](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/18d45c14-0d65-46fa-8c56-25111e22804b)

## 9. Run Synthesis"， "Run Implementation"，Generate Bitstream 後再輸出 Block design

![12](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/c473aa9d-6000-4576-9e42-7cd073cb0c23)

## 10.  輸出.tcl file
* 在Tcl console 中輸入 "write_bd_tcl "你的專案路徑/design_1.tcl" "

![image](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/c96ed45e-63a8-452a-845c-c25ae60b9872)

## 11. 將 design_1.tcl ，design_1_wrapper.bit，design_1.hwh 複製到板子上

* **所有的檔名要一樣**
  
![image](https://github.com/Anderson991288/PYNQ-Z2-note/assets/68816726/95cd8a44-5d2a-4266-a65c-e86ffd2300ea)

## 12. 到Jupyter Notenook 中編寫程式，導入自訂的Overlay

```
import numpy as np
from pynq import allocate
from pynq import Overlay

overlay = Overlay("myip.bit")

hex(overlay.ip_dict['myip_0']['phys_addr'])
adr = overlay.ip_dict['myip_0']['phys_addr']
from pynq import MMIO
leds = MMIO(adr,0xF)
leds.write(0,0x3)

from time import sleep
for i in range(10):
    for j in range(16):
        leds.write(0,j)
        sleep(0.5)
```


* Reference: https://www.youtube.com/watch?v=Ii9JeVHCv_o
