# 机器学习硬件
## 机器学习硬件的Metrix和roofline
### 衡量计算性能
- FLOPs/s 每秒能计算的浮点数
  - 专门进行推理inference的芯片/集成电路常用OPS，因为推理阶段常常降低精度，拥有更高性能和效率。
  - 比如说用8 bits，指的是整数格式，不是浮点数，所以是OPS。
- MACs/s
  - （Multiply-Accumulate Operations，乘累加操作）
  - 一个MAC操作等于2个FLOPS操作，所以MAC数常常等于一半的FLOPS
- Clock cycle
  - 每秒运算速度
  - cycle是指一个时钟循环，电脑执行任务的最小单位
  - PE的数量*每秒的cycles数量\*每个cycle可以做的运算数量
  - PE是processing element，现在并行的系统芯片一般有很多PE

### 衡量memory
- 容量
- 带宽
- 内存等级 hierarchy：caches， local memory有更大的带宽

### 衡量方法-roofline
![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/d110b0df-6923-41a4-8270-6b4a33d5dc1e)
横轴衡量应用的计算密度，而不是硬件的属性  

在横轴最开始意味着，比如说需要1G的数据，做一次运算，然后传回，是bandwidth bound  

在横轴过了交汇点，比如说要取1MB的数据，做十次运算吗，这时候限制是compute bound  

实现的性能一般受限于：
- 一般难以达到横着的那条线因为有一些关于caching缓存的开销
- 我的channel数量不是64的倍数，但是我的芯片上计算元件的数量是64，这样就有一些浪费。
- 关于外部内存的还有，从随机地址loading数据，会导致到达不了带宽峰值。一直从连续的内存地址加载才能实现100%带宽，因为随机的地址不能被打包进一个缓存行cache line。如果这样的话可以一行一行地读取数据。
- 复杂的DNN拓扑结构可能不能完全用芯片

![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/7fd71fc7-1b14-4b3f-842b-9d9e618aa102)  

MLP0这个应用在不同的处理器上有不同的operational intensity，原因：不同的处理器支持的精度不同。 

### Metrix
  
![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/890a5eed-e6b4-4aae-9245-674b4263b454)

### latency延迟和throuhput吞吐量
latency是指运行一个inference有多快，用多少秒完成
throughput是每秒能运行几个inference  
![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/28ee56ee-d0cc-429c-9f18-5d6db82da492)  

batching举例：两个人同时request要获取天气数据，我可以并行处理inference，1秒进行了6次inf但是一个inf花了0.33秒。吞吐量大的时候，延迟也高。

## 硬件的效率提升方法

![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/187c4b93-b2a9-4356-bf0d-13f47f33a096)  

1. 算数优化
  - specialied instructions： amortize overhead 分摊开销
  - 更低精度 即quantization量化
2. 内存优化
  -  把数据存在片上内存
  -  复用，避免fetches
3. 跳过无效计算
  - 比如说最后乘0，如何跳过这些无效计算
4. 模型优化

### 方法1：Amortize overhead
因为一个add指令要花费很多能量在别的辅助功能上，所有一次执行很多个add instruction更加划算。
![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/046b4dcd-662d-47f4-9051-5ec6c1d146c6)

![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/d1c927a5-bcb5-4028-a2d5-94e16524828f)  

### 方法2：数字格式和精度
浮点数vs整数，双精度64 bits，单精度32 bits。

【float32】1个bit表示符号，8个bit表示指数，23个bit表示mantissa小数部分。8个bit可以表示从指数从-126到127.

【float16】1个符号，5个指数，10个小数

【8bit整数】有符号的话-(2^7-1) to 2^7，如果没符号可以表示0-255

浮点数的加法，要对齐指数然后才能相加，增加浮点精度成本会翻很多倍。而浮点数乘法可以并行计算指数部分和小数部分。

### 方法3： locality
SRAM-静态存储器 static random-access memory，有较高功耗要维持存储的数据，保持数据一致性；DRAM动态的，周期性刷新。
![image]![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/979c664c-d8be-4d72-acb0-50291ef5b74a)

![image](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/18c63413-c5fa-435b-a6ec-89be00ad2a9e)

### 方法4： reuse
复用距离，每隔多少次复用一下。想象一个filter要重复用很多次。  
大的复用距离导致我需要设计更多的registers closed to my PE.每个register寄存器都可以存储一个数据元素，一个字或者一个字节。



