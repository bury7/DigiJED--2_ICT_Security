<p align="center">
  # Звіт з лабораторної роботи № 1 з курсу “DigiJED - 2 ICT Security”
**Виконав:** Ніживенко А. Д. 

**Варіант:** № 9

**Харків**

**2023**

</p>
### **Тема:** “Побудова віртуальних локальних мереж”

### **Мета:** Набуття практичних навичок щодо побудови і захисту віртуальних локальних мереж.

### **Вихідні дані:** 
**Перша мережа (first net):** 
2 комутатори з’єднані між собою (Switch 0, Switch 1), сервер під’єднаний до комутатора Switch 0 (Server 0), 2 комп’ютери під’єднані до Switch 1 (PC 0, PC 1), 2 точки доступу (Access Point 1, Access Point 2) під’єднані до Switch 0, 4 ноутбуки під’єднані до Access Point 1 (Laptop 0, Laptop 1, Laptop 2, Laptop 3), 2 ноутбуки під’єднані до Access Point 2 ( Laptop 4, Laptop 5), маршрутизатор під’єднаний до Switch  0 (Router 1).

**Друга мережа (second net):**
комутатор (Switch 2), 3 комп’ютери під’єднані до Switch 2 (PC 2, PC 3, PC 4), сервер під’єднаний до Switch 2 (Server 1), маршрутизатор під’єднаний до Switch  2 (Router 2).

**Третя мережа (third net):** 
2 роутери з’єднані між собою (Router 1, Router 2).

## Хід роботи
1 ) Розділено першу мережу на дві віртуальні мережі: перша з назвою VLAN X  і номером 100, а друга з назвою VLAN Y і номером 200. До VLAN  X входить PC 0, PC 1, Laptop 4 та Laptop 5. До VLAN Y входить Laptop 0, Laptop 1, Laptop 2, Laptop 3 та Server 0.
Для цього виконано такі команди на Switch 0 та Switch 1:

Switch#enable
Switch#configure terminal
Switch(config)#vlan 100
Switch(config-vlan)#name VLAN_X
Switch(config-vlan)#exit
Switch(config)#vlan 200
Switch(config-vlan)#name VLAN_Y
Switch(config-vlan)#exit

2) Розподілено порти доступу на комутаторах: у Switch 0 це порти FastEthernet 0/1 і FastEthernet 0/3 для VLAN Y та FastEthernet 0/4 для VLAN X, у Switch 1 це порти FastEthernet 0/1 і FastEthernet 0/2 для VLAN X.
Для цього виконано такі команди:

на Switch 0:
Switch#enable
Switch#configure terminal
Switch(config)#interface range fastEthernet 0/1, fastEthernet 0/3
Switch(config-if-range)#switchport mode access 
Switch(config-if-range)#switchport access vlan 200
Switch(config-if-range)#exit
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 100
Switch(config-if)#exit

на Switch 1:
Switch#enable
Switch#configure terminal
Switch(config)#interface range fastEthernet 0/1, fastEthernet 0/2
Switch(config-if-range)#switchport mode access 
Switch(config-if-range)#switchport access vlan 100
Switch(config-if-range)#exit

3) Створено транкові канали зв’язку: між Switch 0 (FastEthernet 0/5) та Router 0 (GigabitEthernet 0/0) без використання протоколу DTP, між Switch 0 (FastEthernet 0/2) та Switch 1 (FastEthernet 0/3) з використання протоколу DTP. 
Для цього виконано такі команди:

на Switch 0:
Switch#enable
Switch#configure terminal
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode trunk 
Switch(config-if)#exit
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode dynamic desirable
Switch(config-if)#exit

На Switch 1:
Switch#enable
Switch#configure terminal
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport mode dynamic auto
Switch(config-if)#exit

4) Поділено першу мережу на дві логічні мережі: перша для VLAN X має IP адресу мережі 192.168.10.0/29 та шлюз за замовчуванням в цій мережі знаходиться за адресою 192.168.10.6/29, друга для VLAN Y має IP адерсу мережі 192.168.10.128/29 та шлюз за замовчуванням в цій мережі знаходиться за адресою 192.168.10.134/29.

5) Статично призначено всім пристроям IP адреси у межах мереж, які створено на кроці 4, а саме:

VLAN X:
PC 0 - 192.168.10.1/29,
PC 1 - 192.168.10.2/29,
Laptop 4 - 192.168.10.3/29,
Laptop 5 - 192.168.10.4/29.

VLAN Y: 
Server 0 - 192.168.10.129/29,
Laptop 0 - 192.168.10.130/29,
Laptop 1 - 192.168.10.131/29,
Laptop 2 - 192.168.10.132/29,
Laptop 3 - 192.168.10.133/29.

6) На маршрутизаторі Router 1 налаштовано маршрутизацію між VLAN X та VLAN Y. 
Для цього виконано такі команди на Router 1:

Router>enable
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#no ip address
Router(config-if)#exit
Router(config)#interface gigabitEthernet 0/0.100
Router(config-subif)#encapsulation dot1Q 100
Router(config-subif)#ip address 192.168.10.6 255.255.255.248
Router(config-if)#exit
Router(config)#interface gigabitEthernet 0/0.200
Router(config-subif)#encapsulation dot1Q 200
Router(config-subif)#ip address 192.168.10.134 255.255.255.248
Router(config-if)#exit

7) Виконано налаштування статичних IP адрес на маршрутизаторах Router 1 та Router 2.
Для цього виконано такі команди:

на Router 1:
Router>enable
Router#configure terminal 
Router(config)#ip route 192.168.11.0 255.255.255.0 192.168.254.2


на Router 2:
Router>enable
Router#configure terminal 
Router(config)#ip route 192.168.10.0 255.255.255.248 192.168.254.1
Router(config)#ip route 192.168.10.128 255.255.255.248 192.168.254.1

8) Перевірено зв’язок у віртуальній локальній мережі (Laptop 2 і Server 0):

C:\>ping 192.168.10.129
Pinging 192.168.10.129 with 32 bytes of data:
Reply from 192.168.10.129: bytes=32 time=63ms TTL=128
Reply from 192.168.10.129: bytes=32 time=24ms TTL=128
Reply from 192.168.10.129: bytes=32 time=37ms TTL=128
Reply from 192.168.10.129: bytes=32 time=12ms TTL=128
Ping statistics for 192.168.10.129:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 12ms, Maximum = 63ms, Average = 34ms

C:\>tracert 192.168.10.129
Tracing route to 192.168.10.129 over a maximum of 30 hops:
1 28 ms 27 ms 6 ms 192.168.10.129
Trace complete.

Перевірено зв’язок між віртуальними локальними мережами(Laptop 0 і PC 0):
C:\>ping 192.168.10.1
Pinging 192.168.10.1 with 32 bytes of data:
Request timed out.
Reply from 192.168.10.1: bytes=32 time=8ms TTL=127
Reply from 192.168.10.1: bytes=32 time=39ms TTL=127
Reply from 192.168.10.1: bytes=32 time=18ms TTL=127
Ping statistics for 192.168.10.1:
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
Minimum = 8ms, Maximum = 39ms, Average = 21ms
C:\>tracert 192.168.10.1
Tracing route to 192.168.10.1 over a maximum of 30 hops:
1 27 ms 6 ms 6 ms 192.168.10.134
2 21 ms 22 ms 28 ms 192.168.10.1
Trace complete.

Перевірено зв’язок між першою та другою мережею (Laptop 4 і PC 4):
C:\>ping 192.168.11.12
Pinging 192.168.11.12 with 32 bytes of data:
Request timed out.
Request timed out.
Reply from 192.168.11.12: bytes=32 time=42ms TTL=126
Reply from 192.168.11.12: bytes=32 time=24ms TTL=126
Ping statistics for 192.168.11.12:
Packets: Sent = 4, Received = 2, Lost = 2 (50% loss),
Approximate round trip times in milli-seconds:
Minimum = 24ms, Maximum = 42ms, Average = 33ms

C:\>tracert 192.168.11.12
Tracing route to 192.168.11.12 over a maximum of 30 hops:
1 34 ms 6 ms 34 ms 192.168.10.6
2 30 ms 9 ms 22 ms 192.168.254.2
3 26 ms 29 ms 7 ms 192.168.11.12
Trace complete.

Перевірено конфігурацію Switch 1:
Switch#enable

Switch#show ip interface brief
Interface IP-Address OK? Method Status Protocol
FastEthernet0/1 unassigned YES manual up up
FastEthernet0/2 unassigned YES manual up up
FastEthernet0/3 unassigned YES manual up up
FastEthernet0/4 unassigned YES manual administratively down down
FastEthernet0/5 unassigned YES manual administratively down down
FastEthernet0/6 unassigned YES manual administratively down down
FastEthernet0/7 unassigned YES manual administratively down down
FastEthernet0/8 unassigned YES manual administratively down down
FastEthernet0/9 unassigned YES manual administratively down down
FastEthernet0/10 unassigned YES manual administratively down down
FastEthernet0/11 unassigned YES manual administratively down down
FastEthernet0/12 unassigned YES manual administratively down down
FastEthernet0/13 unassigned YES manual administratively down down
FastEthernet0/14 unassigned YES manual administratively down down
FastEthernet0/15 unassigned YES manual administratively down down
FastEthernet0/16 unassigned YES manual administratively down down
FastEthernet0/17 unassigned YES manual administratively down down
FastEthernet0/18 unassigned YES manual administratively down down
FastEthernet0/19 unassigned YES manual administratively down down
FastEthernet0/20 unassigned YES manual administratively down down
FastEthernet0/21 unassigned YES manual administratively down down
FastEthernet0/22 unassigned YES manual administratively down down
FastEthernet0/23 unassigned YES manual administratively down down
FastEthernet0/24 unassigned YES manual administratively down down
GigabitEthernet0/1 unassigned YES manual administratively down down
GigabitEthernet0/2 unassigned YES manual administratively down down
Vlan1 unassigned YES manual administratively down down

Switch#show interface trunk
Port Mode Encapsulation Status Native vlan
Fa0/3 auto n-802.1q trunking 1
Port Vlans allowed on trunk
Fa0/3 1-1005
Port Vlans allowed and active in management domain
Fa0/3 1,100,200
Port Vlans in spanning tree forwarding state and not pruned
Fa0/3 1,100,200

Switch#show vlan brief
VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/4, Fa0/5, Fa0/6, Fa0/7
Fa0/8, Fa0/9, Fa0/10, Fa0/11
Fa0/12, Fa0/13, Fa0/14, Fa0/15
Fa0/16, Fa0/17, Fa0/18, Fa0/19
Fa0/20, Fa0/21, Fa0/22, Fa0/23
Fa0/24, Gig0/1, Gig0/2
100 VLAN_X active Fa0/1, Fa0/2
200 VLAN_Y active
1002 fddi-default active
1003 token-ring-default active
1004 fddinet-default active
1005 trnet-default active

Switch#show vtp status
VTP Version capable : 1 to 2
VTP version running : 1
VTP Domain Name : first_net
VTP Pruning Mode : Disabled
VTP Traps Generation : Disabled
Device ID : 00D0.D379.C900
Configuration last modified by 0.0.0.0 at 3-1-93 00:40:10
Local updater ID is 0.0.0.0 (no valid interface found)
Feature VLAN :
--------------
VTP Operating Mode : Server
Maximum VLANs supported locally : 255
Number of existing VLANs : 7
Configuration Revision : 7
MD5 digest : 0x91 0x5D 0x5C 0x2B 0x5E 0x8A 0xAC 0x7F
0x26 0x8C 0xAD 0x56 0xC0 0xD0 0x9F 0x93

Перевірено конфігурацію Switch 0:
Switch>enable

Switch#show ip interface brief
Interface IP-Address OK? Method Status Protocol
FastEthernet0/1 unassigned YES manual up up
FastEthernet0/2 unassigned YES manual up up
FastEthernet0/3 unassigned YES manual up up
FastEthernet0/4 unassigned YES manual up up
FastEthernet0/5 unassigned YES manual up up
FastEthernet0/6 unassigned YES manual administratively down down
FastEthernet0/7 unassigned YES manual administratively down down
FastEthernet0/8 unassigned YES manual administratively down down
FastEthernet0/9 unassigned YES manual administratively down down
FastEthernet0/10 unassigned YES manual administratively down down
FastEthernet0/11 unassigned YES manual administratively down down
FastEthernet0/12 unassigned YES manual administratively down down
FastEthernet0/13 unassigned YES manual administratively down down
FastEthernet0/14 unassigned YES manual administratively down down
FastEthernet0/15 unassigned YES manual administratively down down
FastEthernet0/16 unassigned YES manual administratively down down
FastEthernet0/17 unassigned YES manual administratively down down
FastEthernet0/18 unassigned YES manual administratively down down
FastEthernet0/19 unassigned YES manual administratively down down
FastEthernet0/20 unassigned YES manual administratively down down
FastEthernet0/21 unassigned YES manual administratively down down
FastEthernet0/22 unassigned YES manual administratively down down
FastEthernet0/23 unassigned YES manual administratively down down
FastEthernet0/24 unassigned YES manual administratively down down
GigabitEthernet0/1 unassigned YES manual administratively down down
GigabitEthernet0/2 unassigned YES manual administratively down down
Vlan1 unassigned YES manual administratively down down

Switch#show interface trunk
Port Mode Encapsulation Status Native vlan
Fa0/2 desirable n-802.1q trunking 1
Fa0/5 on 802.1q trunking 1
Port Vlans allowed on trunk
Fa0/2 1-1005
Fa0/5 1-1005
Port Vlans allowed and active in management domain
Fa0/2 1,100,200
Fa0/5 1,100,200
Port Vlans in spanning tree forwarding state and not pruned
Fa0/2 1,100,200
Fa0/5 1,100,200

Switch#show vlan brief
VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/6, Fa0/7, Fa0/8, Fa0/9
Fa0/10, Fa0/11, Fa0/12, Fa0/13
Fa0/14, Fa0/15, Fa0/16, Fa0/17
Fa0/18, Fa0/19, Fa0/20, Fa0/21
Fa0/22, Fa0/23, Fa0/24, Gig0/1
Gig0/2
100 VLAN_X active Fa0/4
200 VLAN_Y active Fa0/1, Fa0/3
1002 fddi-default active
1003 token-ring-default active
1004 fddinet-default active
1005 trnet-default active

Switch#show vtp status
VTP Version capable : 1 to 2
VTP version running : 1
VTP Domain Name : first_net
VTP Pruning Mode : Disabled
VTP Traps Generation : Disabled
Device ID : 00E0.F954.6700
Configuration last modified by 0.0.0.0 at 3-1-93 00:40:10
Local updater ID is 0.0.0.0 (no valid interface found)
Feature VLAN :
--------------
VTP Operating Mode : Server
Maximum VLANs supported locally : 255
Number of existing VLANs : 7
Configuration Revision : 7
MD5 digest : 0x91 0x5D 0x5C 0x2B 0x5E 0x8A 0xAC 0x7F
0x26 0x8C 0xAD 0x56 0xC0 0xD0 0x9F 0x93


Перевірено конфігурацію Router 1:
Router>enable

Router#show ip interface brief
Interface IP-Address OK? Method Status Protocol
GigabitEthernet0/0 unassigned YES manual up up
GigabitEthernet0/0.100 192.168.10.6 YES manual up up
GigabitEthernet0/0.200 192.168.10.134 YES manual up up
GigabitEthernet0/1 192.168.254.1 YES manual up up
GigabitEthernet0/2 unassigned YES unset administratively down down
Vlan1 unassigned YES unset administratively down down

Router#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
* - candidate default, U - per-user static route, o - ODR
P - periodic downloaded static route
Gateway of last resort is not set
192.168.10.0/24 is variably subnetted, 4 subnets, 2 masks
C 192.168.10.0/29 is directly connected, GigabitEthernet0/0.100
L 192.168.10.6/32 is directly connected, GigabitEthernet0/0.100
C 192.168.10.128/29 is directly connected, GigabitEthernet0/0.200
L 192.168.10.134/32 is directly connected, GigabitEthernet0/0.200
S 192.168.11.0/24 [1/0] via 192.168.254.2
192.168.254.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.254.0/24 is directly connected, GigabitEthernet0/1
L 192.168.254.1/32 is directly connected, GigabitEthernet0/1

Перевірено конфігурацію Router 2:
Router>enable

Router#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
* - candidate default, U - per-user static route, o - ODR
P - periodic downloaded static route
Gateway of last resort is not set
192.168.10.0/29 is subnetted, 2 subnets
S 192.168.10.0/29 [1/0] via 192.168.254.1
S 192.168.10.128/29 [1/0] via 192.168.254.1
192.168.11.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.11.0/24 is directly connected, GigabitEthernet0/0
L 192.168.11.254/32 is directly connected, GigabitEthernet0/0
192.168.254.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.254.0/24 is directly connected, GigabitEthernet0/1
L 192.168.254.2/32 is directly connected, GigabitEthernet0/1

9) У склад мережі до Switch 0 дододано комутатор (Switch Enemy), на цьому пристрої було увімкнено протокол DTP для створення транкового з’єднання між Switch Enemy та Switch 0. Для цього виконано такі команди на Switch Enemy:
Switch#enable
Switch#configure terminal
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode dynamic desirable

Для запобігання впливу цього комутатора через протокол VTP було встановлено доменне ім’я first_net та пароль для VTP. 
Для цього виконано такі команди на Switch 0 та Switch 1:
Switch>enable
Switch#configure terminal 
Switch(config)#vtp domain first_net
Switch(config)#vtp password eXgCwqsqY5jC

Також на маршрутизаторах було вимкнено порти, які не використовуються.
Для цього виконані такі команди:
на Switch 0:
Router>enable
Router#configure terminal 
Switch(config)#interface range fastEthernet 0/6 - 24
Switch(config-if-range)#shutdown
Switch(config-if-range)#exit
Switch(config)#interface range gigabitEthernet 0/1 - 2
Switch(config-if-range)#shutdown 

на Switch 1:
Router>enable
Router#configure terminal 
Switch(config)#interface range fastEthernet 0/4 - 24
Switch(config-if-range)#shutdown
Switch(config-if-range)#exit
Switch(config)#interface range gigabitEthernet 0/1 - 2
Switch(config-if-range)#shutdown 

Висновок: 
Під час цієї лабораторної роботи були отримані навички створення і захисту віртуальних локальних мереж: навчились створювати порти доступу і транкові порти, конфігурувати маршрутизатор для маршрутизації між віртуальними локальними мережами. Досліджено протокол DTP та VTP, їх недоліки та переваги (автоматизація це дуже зручно і полегшує роботу, але є ризик, що зловмисники можуть знайти і використати недоліки цих протоколів). 


