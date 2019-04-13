是一个将websocket转成tcp的代理，用了此代理之后，可以直接用原来的tcp服务器，然后客户端用websocket进行通信。
this can receive websocket from one client then connect to a server by tcp , then transfer msg between them.

用法 usage:  websocket_transfer_tcp   8888    0.0.0.0:8889   (客户端用websocket连接8889端口，然后转移到8888端口的tcp服务器)

代理性能测试：

测试机器： 腾讯云 4核4G内存，go版本1.5.1

初次测试： 1W连接，连续不断发消息，会到6W+ /s 读写次数，si45%左右，（每秒10个左右的连接会等待超过2秒，其他都很快）

		  之后改成每个连接都是1秒发送一次消息则可以将连接增加到2W8左右无压力，之后出现连接不上的情况。
		  
机器调优： 发现是可用端口已经到达极限（linux默认可用端口为32768-61000） 

		  修改可用端口：   /etc/sysctl.conf
		  
		  net.ipv4.ip_local_port_range = 1024 65535
		  
		  sysctl -p
		  
最终测试： 最后可到6W左右连接，每个连接1S发送一条数据， 读写为6Ｗ/s si有到70% 左右 (每秒30个所有的连接会等到超过2S）


因没有做连接收缩，所以只能到6W连接，此时压力也比较正常。

