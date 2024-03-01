# VirtualBox install Ubuntu
# 1.解决ubuntu安装问题：
![71cf1381f53a319d5b7cc54c8fb21ba](https://github.com/JoyJoyWang/algorithm_notes/assets/67251304/95358063-620e-4b41-928b-913a445d6aeb)
原因：Ryzen移动的7x40芯片出现早期内核故障 （除以零）由于不存在输入验证 从amd_cpuid4（）中的CPUID数据。
解决办法：
打开命令提示符，输入
```
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" setextradata  YOUR_VM_NAME VBoxInternal/CPUM/HostCPUID/80000006/edx 0x02009140   
```
其中YOUR_VM_NAME是你创建的虚拟机的名称
参考：[link](https://www.reddit.com/r/ZephyrusG14/comments/14icobc/linux_virtual_machine/)


# 2. 解决ubuntu与windows之间不能互相复制的问题
首先要在菜单栏的“设备”-“网络”中启动网络连接，然后：
```
$sudo apt-get install virtualbox-guest-x11
$VBoxClient --clipboard
```
# 3. 解决共享文件夹no permission问题
把自己添加到这个vboxsf组里面：  
```
sudo usermod -a -G vboxsf 当前用户名
```
