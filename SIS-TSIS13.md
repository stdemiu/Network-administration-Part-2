# 13.	Управление сетевой инфраструктурой:
СРО: Настройте сетевое оборудование (например, маршрутизатор и коммутатор Cisco) для создания виртуальных локальных сетей (VLAN) и настройки динамической маршрутизации (OSPF). Подготовьте отчет о конфигурации и тестировании.
СРОП: Выполните аудит текущей сетевой инфраструктуры компании (или симулированной сети). Оцените производительность и отказоустойчивость инфраструктуры, предоставьте отчет с рекомендациями по улучшению.

## 1. Настройка сетевого оборудования
### 1.1. Создание VLAN и настройка OSPF на маршрутизаторе и коммутаторе Cisco
#### Создание VLAN на коммутаторе Cisco

1. Создайте VLAN 10 (для отдела IT) и VLAN 20 (для отдела Sales):  
   ```plaintext
   Switch> enable
   Switch# configure terminal
   Switch(config)# vlan 10
   Switch(config-vlan)# name IT
   Switch(config-vlan)# exit
   Switch(config)# vlan 20
   Switch(config-vlan)# name Sales
   Switch(config-vlan)# exit
   ```

2. Назначьте интерфейсы VLAN:  
   ```plaintext
   Switch(config)# interface FastEthernet0/1
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit
   Switch(config)# interface FastEthernet0/2
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 20
   Switch(config-if)# exit
   ```

3. Включите транк для связи между коммутатором и маршрутизатором:  
   ```plaintext
   Switch(config)# interface FastEthernet0/24
   Switch(config-if)# switchport mode trunk
   Switch(config-if)# exit
   ```

#### Настройка OSPF на маршрутизаторе Cisco
1. Включите OSPF и настройте сеть:  
   ```plaintext
   Router> enable
   Router# configure terminal
   Router(config)# router ospf 1
   Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
   Router(config-router)# network 192.168.2.0 0.0.0.255 area 0
   Router(config-router)# exit
   ```

2. Проверьте работу OSPF:  
   ```plaintext
   Router# show ip ospf neighbor
   Neighbor ID     Pri   State           Dead Time   Address         Interface
   192.168.1.2     1     FULL/BDR        00:00:30    192.168.1.1     FastEthernet0/1
   ```

## 2. Аудит сетевой инфраструктуры
### 2.1. Анализ производительности
Используйте `iperf3` для проверки пропускной способности:  
1. Установите `iperf3` на сервере и клиенте:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo apt install iperf3
   ```
2. Запустите сервер на одной стороне:  
   ```bash
   stacy-virtualbox:/home/stacy# iperf3 -s
   ```
3. Проверьте производительность с клиентской стороны:  
   ```bash
   stacy-virtualbox:/home/stacy# iperf3 -c 192.168.1.1
   ```
   **Вывод:**  
   ```plaintext
   [ ID] Interval           Transfer     Bandwidth
   [  4]   0.00-10.00 sec   1.10 GBytes  944 Mbits/sec
   ```
   
### 2.2. Оценка отказоустойчивости
1. Проверьте отказ одного из интерфейсов маршрутизатора:  
   ```plaintext
   Router(config)# interface FastEthernet0/1
   Router(config-if)# shutdown
   ```

2. Убедитесь, что OSPF маршрутизирует трафик через резервный путь:  
   ```plaintext
   Router# show ip route
   Gateway of last resort is 192.168.2.1 to network 0.0.0.0
   ```

## 3. Улучшения
### 3.1. VLAN и OSPF
- Убедитесь, что VLAN корректно разделяют сетевой трафик для изоляции данных.  
- Настройте резервные соединения между маршрутизаторами для повышения отказоустойчивости.  
### 3.2. Производительность и отказоустойчивость
- Используйте инструменты мониторинга, такие как Zabbix или Nagios, для анализа трафика и выявления узких мест.  
- Настройте резервные маршруты и балансировку нагрузки для повышения устойчивости сети.  
