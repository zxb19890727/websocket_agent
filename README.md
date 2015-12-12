# websocket_transfer
this can receive websocket from one client then connect to a server by tcp , then transfer msg between the two.

usage:
websocket_transfer_tcp 8888 0.0.0.0:8889

测试数据

机器性能：
4核 3G内存
测试时go的版本为1.5.1


1W连接， 6W+ /s 读写次数， 4核， si在45% 左右， （每秒有10个左右的连接会等到接受超过2S）
增加到1W5连接， 还是6W/s  读写次数  si有到70% 左右 (每秒40个所有的连接会等到超过2S， 偶尔会3S)


每个连接改成1S发一条消息  1Ｗ连接没有任何压力，　１Ｗ/s的读写速度， 20%左右的si
1W5 si最高的20%左右

初始最高连接在2W8左右， 所以后边比较困难进行连接

可以修改
vi /etc/sysctl.conf   

添加下面一行： 

net.ipv4.ip_local_port_range = 1024 65535

sysctl -p 

Linux默认的可用端口范围是： 32768-61000

引用
[root@PerfTestApp3 ~]# sysctl -a|grep ip_local_port_range 
net.ipv4.ip_local_port_range = 32768    61000

将默认端口修改为1024-65535

这样就会使得默认可用端口为1024-65535

替换之后再次进行测试
每个连接1S发送一条数据， 读写为6Ｗ/s   si有到70% 左右 (每秒30个所有的连接会等到超过2S）

