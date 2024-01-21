# Creating a custom AXI Lite IP in Vivado 

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
