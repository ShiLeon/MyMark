## # Verilog

### 时间(time)`$time//当前时间`

### 阻塞赋值与非阻塞赋值

在设计电路时，always 时序逻辑块中多用非阻塞赋值，always 组合逻辑块中多用阻塞赋值；在仿真电路时，initial 块中一般多用阻塞赋值。

### 时延

- 常规时延`#10 value_general = value_test ;` 延时后赋值

- 内嵌式时延`value_embed = #10 value_test ;`先赋值再时延

### 事件

- 命名事件控制
  
  ```verilog
  event     start_receiving ;
  always @( posedge clk_samp) begin
          -> start_receiving ;       //采样时钟上升沿作为时间触发时刻
  end
  
  always @(start_receiving) begin
      data_buf = {data_if[0], data_if[1]} ; //触发时刻，对多维数据整合
  end
  ```

### 竞争与冒险

- 增加滤波电容，滤除窄脉冲

- 修改逻辑，增加冗余项

- 使用时钟同步电路，利用触发器进行打拍延迟

- 采用格雷码计数器

## FIFO

用于读写速率不一致时

## SD

挂载时要注意盘符

`"1:/s.txt"，"0:/s.txt"`

1指的是SD1，0指的是SD0

黑金的板子SD1是SD卡，SD0是EMMC

## 接口设计

+ 片上存储器接口（使用bram）

+ 外部存储器接口（使用dram）

+ 数据流接口
