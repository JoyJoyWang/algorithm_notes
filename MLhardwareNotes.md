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




