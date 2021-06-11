# ecjtu-network-ExtraLab6
华东交通大学计算机网络 最终实验六：校园网

>实验指导书给出的拓扑图：
>  ![extra](https://user-images.githubusercontent.com/57565901/121632559-c5b71980-cab3-11eb-9dab-2f494ca044cb.jpg)

## 0x01 :我们根据实验给出的拓扑图在Cisco Packet Tracer中把对应的设备选出并摆放好:
如下图：
![image](https://user-images.githubusercontent.com/57565901/121632932-78877780-cab4-11eb-8bc5-0fc253210e30.png)

由于网络结构过于复杂，为了便于读者阅读，我把各区域的拓扑图分开截取：

### 【教学区】
![image](https://user-images.githubusercontent.com/57565901/121634080-905ffb00-cab6-11eb-8b33-f7909d019262.png)
### 【宿舍区】
![image](https://user-images.githubusercontent.com/57565901/121633088-c308f400-cab4-11eb-9582-209eacca2813.png)
### 【骨干网络区】
![image](https://user-images.githubusercontent.com/57565901/121633148-dae07800-cab4-11eb-88d1-66ff2049f87e.png)
### 【外网出口】
![image](https://user-images.githubusercontent.com/57565901/121633187-e9c72a80-cab4-11eb-86b1-d1f63a0c551b.png)
### 【DMZ数据机房区】
![image](https://user-images.githubusercontent.com/57565901/121634113-9ce45380-cab6-11eb-9091-8babf45ce929.png)

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
