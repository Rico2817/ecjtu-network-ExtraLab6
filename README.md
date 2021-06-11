# ecjtu-network-ExtraLab6
华东交通大学计算机网络 最终实验六：校园网

>实验指导书给出的拓扑图：
>  ![extra](https://user-images.githubusercontent.com/57565901/121632559-c5b71980-cab3-11eb-9dab-2f494ca044cb.jpg)

## 0x01 :我们根据实验给出的拓扑图在Cisco Packet Tracer中把对应的设备选出并摆放好:
如下图：
![image](https://user-images.githubusercontent.com/57565901/121632932-78877780-cab4-11eb-8bc5-0fc253210e30.png)

由于网络结构过于复杂，为了便于读者阅读，我把各区域的拓扑图分开截取：

### 【教学区】
![image](https://user-images.githubusercontent.com/57565901/121639715-87bff280-cabf-11eb-9dad-f31e487cea05.png)

### 【宿舍区】
![image](https://user-images.githubusercontent.com/57565901/121639752-973f3b80-cabf-11eb-8702-685d4384e9d5.png)
### 【骨干网络区】
![image](https://user-images.githubusercontent.com/57565901/121639824-af16bf80-cabf-11eb-9d5d-617fe34728ab.png)
### 【外网出口】
![image](https://user-images.githubusercontent.com/57565901/121639841-b938be00-cabf-11eb-83e1-63684f34c743.png)
### 【DMZ数据机房区】
![image](https://user-images.githubusercontent.com/57565901/121639890-c786da00-cabf-11eb-8117-65ad5262362f.png)

## 0x02:配置接口和IP，设置网关：
PC1(静态IP主机以此类推,DHCP协议主机例外):

![image](https://user-images.githubusercontent.com/57565901/121633542-90133000-cab5-11eb-8d52-1065bc87f32a.png)
### 0x0201: 各种二层路由器的配置：
>配置交换机接口Vlan并打开接口
Switch 0:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 500
% Access VLAN does not exist. Creating vlan 500
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 500
```
Switch 1:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 501
% Access VLAN does not exist. Creating vlan 501
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 501
```
Switch 2:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 502
% Access VLAN does not exist. Creating vlan 502
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 502
```
Switch 3:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 700
% Access VLAN does not exist. Creating vlan 700
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 700
```
Switch 4:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 701
% Access VLAN does not exist. Creating vlan 701
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 701
```
Switch 5:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/24
Switch(config-if)#switchport access vlan 702
% Access VLAN does not exist. Creating vlan 702
Switch(config-if)#int fa0/1 
Switch(config-if)#switchport access vlan 702
```

因为Switch 6的配置和上面六个交换机的配置有所不同，
所以我决定把Switch 6的配置放在数据中心区配置里单独介绍。
下面是每个区域内设备的简单配置。
（暂时不配置路由策略，路由策略放到后面）
### 0x0201: 数据中心区:
Switch 6:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/1
Switch(config-if)#switchport access vlan 101
% Access VLAN does not exist. Creating vlan 101
Switch(config-if)#int fa0/2
Switch(config-if)#switchport access vlan 102
% Access VLAN does not exist. Creating vlan 102
Switch(config-if)#int fa0/3
Switch(config-if)#switchport access vlan 103
% Access VLAN does not exist. Creating vlan 103
Switch(config-if)#int fa0/4
Switch(config-if)#switchport access vlan 104
% Access VLAN does not exist. Creating vlan 104
Switch(config-if)#int fa0/24
Switch(config-if)#switchport mode trunk 
```
Multilayer Switch 3:
```shell
Switch>en
Switch#conf t
Switch(config)#int vlan 101
Switch(config-if)#ip address 192.168.1.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int vlan 102
Switch(config-if)#ip address 192.168.2.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int vlan 103
Switch(config-if)#ip address 192.168.3.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int vlan 104
Switch(config-if)#ip address 192.168.4.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/10
Switch(config-if)#switchport access vlan 101
Switch(config-if)#switchport access vlan 102
Switch(config-if)#switchport access vlan 103
Switch(config-if)#switchport access vlan 104
```
这里的目的是让各主机/服务器能ping通这个三层交换机MS3：

![image](https://user-images.githubusercontent.com/57565901/121637122-842a6c80-cabb-11eb-96c1-5d18dbb5b79a.png)

看来我们成功了！下面我们继续教学区(●'◡'●)
### 0x0202: 教学区:
Multilayer Switch 0:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/1
Switch(config-if)#switchport access vlan 500
Switch(config-if)#int vlan 500
Switch(config-if)#ip address 172.16.10.1 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/2
Switch(config-if)#switchport access vlan 501
Switch(config-if)#int vlan 501
Switch(config-if)#ip address 172.16.20.1 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/3
Switch(config-if)#switchport access vlan 502
Switch(config-if)#int vlan 502
Switch(config-if)#ip address 172.16.30.1 255.255.255.0
Switch(config-if)#no shut
```
成功截图：

![image](https://user-images.githubusercontent.com/57565901/121637797-8b9e4580-cabc-11eb-9363-2b6b1945f9bb.png)
### 0x0203: 学生宿舍区(DHCP动态分配IP):
Multilayer Switch 4(以下简称MS4):
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/1
Switch(config-if)#switchport access vlan 700
Switch(config-if)#int vlan 700
Switch(config-if)#ip address 172.20.40.1 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/2
Switch(config-if)#switchport access vlan 701
Switch(config-if)#int vlan 701
Switch(config-if)#ip address 172.20.50.1 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/3
Switch(config-if)#switchport access vlan 702
Switch(config-if)#int vlan 702
Switch(config-if)#ip address 172.20.60.1 255.255.255.0
Switch(config-if)#no shut
```
MS4配置DHCP服务:
```shell
Switch(config)#ip dhcp pool vlan700
Switch(dhcp-config)#network 172.20.40.0 255.255.255.0
Switch(dhcp-config)#defa
Switch(dhcp-config)#default-router 172.20.40.1
Switch(dhcp-config)#dns-server 0.0.0.0
Switch(dhcp-config)#exit
Switch(config)#ip dhcp pool vlan701
Switch(dhcp-config)#network 172.20.50.0 255.255.255.0
Switch(dhcp-config)#default-router 172.20.50.1
Switch(dhcp-config)#dns-server 0.0.0.0
Switch(dhcp-config)#ip dhcp pool vlan702
Switch(dhcp-config)#network 172.20.60.0 255.255.255.0
Switch(dhcp-config)#default-router 172.20.60.1
Switch(dhcp-config)#dns-server 0.0.0.0
Switch(dhcp-config)#exit
```
这几条指令的作用是让所配置的DHCP不分配以下几个IP，因为这几个IP作为网关配置在虚接口Vlan700、701、702中。
```shell
Switch(config)#ip dhcp excluded-address 172.20.40.1
Switch(config)#ip dhcp excluded-address 172.20.50.1
Switch(config)#ip dhcp excluded-address 172.20.60.1
```
注意这里我们要在PC中打开DHCP服务噢！

![image](https://user-images.githubusercontent.com/57565901/121639235-d02ae080-cabe-11eb-86f0-db743c54beb4.png)
出现图上右下角的由DHCP服务分发的IP，说明DHCP服务配置完成！
下面我们测试一下主机PC与MS4的连通性：

![image](https://user-images.githubusercontent.com/57565901/121639330-f9e40780-cabe-11eb-82e7-f75abeba880b.png)
成功！！！我们继续往下做。

### 0x0204: 骨干网络区:
>为了便于读者阅读，我们将三层交换机`Multilayer Switch`缩写为`MS`。
MS0:
```shell
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int f0/24
Switch(config-if)#sw
Switch(config-if)#switchport access vlan 321
% Access VLAN does not exist. Creating vlan 321
Switch(config-if)#int vlan 321
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan321, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan321, changed state to up

Switch(config-if)#ip address 10.10.21.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#
```
MS1:
```shell
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int fa0/24
Switch(config-if)#sw
Switch(config-if)#switchport acc
Switch(config-if)#switchport access vlan 321
% Access VLAN does not exist. Creating vlan 321
Switch(config-if)#int vlan 321
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan321, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan321, changed state to up

Switch(config-if)#ip address 10.10.21.254
% Incomplete command.
Switch(config-if)#ip address 10.10.21.254 255.255.255.0
Switch(config-if)#
Switch(config-if)#int fa0/1
Switch(config-if)#sw
Switch(config-if)#switchport access vlan 320
% Access VLAN does not exist. Creating vlan 320
Switch(config-if)#int vlan 320
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan320, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan320, changed state to up

Switch(config-if)#ip address 10.10.20.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#
```
MS4:
```shell

Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int fa0/24
Switch(config-if)#sw
Switch(config-if)#switchport access vlan 331
% Access VLAN does not exist. Creating vlan 331
Switch(config-if)#int vlan 331
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan331, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan331, changed state to up

Switch(config-if)#ip address 10.10.31.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#
```
MS5:
```shell
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int fa0/24
Switch(config-if)#sw
Switch(config-if)#switchport access vlan 331
% Access VLAN does not exist. Creating vlan 331
Switch(config-if)#int vlan 331
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan331, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan331, changed state to up

Switch(config-if)#ip address 10.10.31.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/1
Switch(config-if)#sw
Switch(config-if)#switchport access vlan 330
% Access VLAN does not exist. Creating vlan 330
Switch(config-if)#int vlan 330
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan330, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan330, changed state to up

Switch(config-if)#ip address 10.10.30.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#
```
>MS2和MS6两台三层交换机要形成聚合链路,所以这里单独分开配置。

MS2:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/2
Switch(config-if)#channel-group 1 mode on
Switch(config-if)#switchport trunk encapsulation dot1Q
Switch(config-if)#switchport mode trunk
Switch(config-if)#int fa0/3
Switch(config-if)#channel-group 1 mode on
Switch(config-if)#switchport trunk encapsulation dot1Q
Switch(config-if)#switchport mode trunk
Switch(config-if)#int Port-channel 1
Switch(config-if)#switchport trunk encapsulation dot1Q
Switch(config-if)#switchport mode trunk
Switch(config-if)#int fa0/1
Switch(config-if)#switchport access vlan 320
Switch(config-if)#int vlan 320
Switch(config-if)#ip address 10.10.20.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int range f0/2-3
Switch(config-if-range)#switchport access vlan 300
Switch(config-if-range)#int vlan 300
Switch(config-if)#ip address 10.10.1.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/24
Switch(config-if)#switchport access vlan 340
Switch(config-if)#int vlan 340
Switch(config-if)#ip address 10.10.40.253 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/10
Switch(config-if)#switchport access vlan 341
Switch(config-if)#int vlan 341
Switch(config-if)#ip address 10.10.60.253 255.255.255.0
Switch(config-if)#no shut
```
MS6:
```shell
Switch>en
Switch#conf t
Switch(config)#int fa0/1
Switch(config-if)#switchport access vlan 330
Switch(config-if)#int vlan 330
Switch(config-if)#ip address 10.10.30.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int range fa0/2-3
Switch(config-if-range)#channel-group 1 mode on
Switch(config-if-range)#switchport trunk encapsulation dot1Q
Switch(config-if-range)#switchport mode trunk
Switch(config-if-range)#switchport access vlan 300
Switch(config-if-range)#int vlan 300
Switch(config-if)#ip address 10.10.1.254 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#int fa0/10
Switch(config-if)#switchport access vlan 350
Switch(config-if)#int vlan 350
Switch(config-if)#ip address 10.10.70.253 255.255.255.0
Switch(config-if)#no shut
```
聚合链路完成！下面我们配置路由器Router0的接口IP和网关。
Router 0:
```shell
Router>en
Router#conf t
Router(config)#int e1/1
Router(config-if)#ip address 10.10.70.254 255.255.255.0
Router(config-if)#no shut
Router(config)#int e1/0
Router(config-if)#ip address 10.10.60.254 255.255.255.0
Router(config-if)#no shut
Router(config-if)#int f0/0
Router(config-if)#ip address 10.10.50.254 255.255.255.0
Router(config-if)#no shut
Router(config-if)#int f0/1
Router(config-if)#ip address 200.1.1.254 255.255.255.0
Router(config-if)#no shut
```
>到这里，我们把全部的接口和网关已经配置好了，万事大吉，只欠东风(路由策略)！
>只要把路由策略做完就能把所有设备联通，
>下面我们就开始我们熟悉的OSPF动态路由的配置！

## 0x03 : OSPF动态路由协议部署

>如图，我将划分的ospf区域标注起来:
>![image](https://user-images.githubusercontent.com/57565901/121667233-a41e5800-cadc-11eb-8b5b-7f1631692e75.png)

MS0:
```shell
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 1.1.1.1 255.255.255.0
Switch(config-if)#no shut 
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 1
Switch(config-router)#router-id 1.1.1.1
Switch(config-router)#log-adjacency-changes 
Switch(config-router)#network 1.1.1.0 0.0.0.255 area 1
Switch(config-router)#network 172.16.0.0 0.0.255.255 area 1
Switch(config-router)#network 10.10.21.0 0.0.0.255 area 1
Switch(config-router)#exit
```
MS1:
```shell
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 2.2.2.2 255.255.255.0
Switch(config-if)#no shut
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 1
Switch(config-router)#router-id 2.2.2.2
Switch(config-router)#network 2.2.2.0 0.0.0.255 area 1
Switch(config-router)#network 10.10.21.0 0.0.0.255 area 1
Switch(config-router)#network 10.10.20.0 0.0.0.255 area 1
Switch(config-router)#exit
Switch(config)#
```
MS4:
```shell
Switch>en
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 5.5.5.5 255.255.255.0
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 2
Switch(config-router)#router-id 5.5.5.5
Switch(config-router)#network 5.5.5.0 0.0.0.255 area 2
Switch(config-router)#network 172.20.0.0 0.0.255.255 area 2
Switch(config-router)#network 10.10.31.0 0.0.0.255 area 2
Switch(config-router)#log-adjacency-changes 
Switch(config-router)#exit
Switch(config)#
```
MS5:
```shell
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 6.6.6.6 255.255.255.0
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 2
Switch(config-router)#router-id 6.6.6.6
Switch(config-router)#network 6.6.6.0 0.0.0.255 area 2
Switch(config-router)#network 10.10.31.0 0.0.0.255 area 2
Switch(config-router)#network 10.10.30.0 0.0.0.255 area 2
Switch(config-router)#log-adjacency-changes 
Switch(config-router)#exit
Switch(config)#
```
>下面配置聚合链路中的OSPF：

MS2:
```shell
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 3.3.3.3 255.255.255.0
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 1
Switch(config-router)#router-id 3.3.3.3 
Switch(config-router)#log
Switch(config-router)#network 3.3.3.0 0.0.0.255 area 0
Switch(config-router)#network 10.10.20.0 0.0.0.255 area 1
Switch(config-router)#network 10.10.40.0 0.0.0.255 area 3
Switch(config-router)#network 10.10.1.0 0.0.0.255 area 0
Switch(config-router)#network 10.10.60.0 0.0.0.255 area 4
Switch(config-router)#exit
Switch(config)#
```

MS6:
```shell
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#
Switch(config)#int loopback0

Switch(config-if)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

Switch(config-if)#ip address 7.7.7.7 255.255.255.0
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 2
Switch(config-router)#router-id 7.7.7.7
Switch(config-router)#log
Switch(config-router)#network 7.7.7.0 0.0.0.255 area 0
Switch(config-router)#network 10.10.30.0 0.0.0.255 area 2
Switch(config-router)#network 10.10.70.0 0.0.0.255 area 4
Switch(config-router)#network 10.10.70.0 0.0.0.255 area 4
01:15:16: %OSPF-5-ADJCHG: Process 2, Nbr 6.6.6.6 on Vlan330 network 
% Incomplete command.
Switch(config-router)#
Switch(config-router)#network 10.10.1.0 0.0.0.255 area 0
Switch(config-router)#exit
Switch(config)#
```
MS3:
```shell
Switch>en
Switch#conf t
Switch(config)#int loopback0
Switch(config-if)#ip address 4.4.4.4 255.255.255.0
Switch(config-if)#exit
Switch(config)#ip routing
Switch(config)#router ospf 3
Switch(config-router)#router-id 4.4.4.4
Switch(config-router)#log
Switch(config-router)#network 4.4.4.0 0.0.0.255 area 3
Switch(config-router)#network 10.10.40.0 0.0.0.255 area 3
Switch(config-router)#network 10.10.50.0 0.0.0.255 area 3
Switch(config-router)#network 192.168.0.0 0.0.255.255 area 3
Switch(config-router)#exit
Switch(config)#
```
Router0:
```shell
Router>en
Router#conf t
Router(config)#int loopback0
Router(config-if)#ip address 8.8.8.8 255.255.255.0
Router(config-if)#exit
Router(config)#router ospf 4
Router(config-router)#router 8.8.8.8
Router(config-router)#router-id 8.8.8.8
Router(config-router)#log
Router(config-router)#network 8.8.8.0 0.0.0.255 area 4
Router(config-router)#network 10.10.60.0 0.0.0.255 area 4
Router(config-router)#network 10.10.70.0 0.0.0.255 area 4
Router(config-router)#network 200.1.1.0 0.0.0.255 area 4
Router(config-router)#network 10.10.50.0 0.0.0.255 area 3
Router(config-router)#exit
```
