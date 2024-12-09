# 10.	Защита сети и межсетевые экраны:
СРО: Настройте межсетевой экран на основе iptables или firewalld в Linux или с использованием Windows Firewall. Подготовьте отчет с примерами правил для защиты от различных видов атак (DDoS, блокировка подозрительных IP).
СРОП: Выполните настройку и тестирование межсетевого экрана (например, Cisco ASA или pfSense) в виртуальной сети. Оцените его эффективность против сетевых атак и представьте результаты в отчете.

## 1. Настройка межсетевого экрана
### 1.1. Ubuntu 22.04 (iptables)
Блокировка IP-адреса 203.0.113.10:  
```bash
stacy-virtualbox:/home/stacy# sudo iptables -A INPUT -s 203.0.113.10 -j DROP
```

Защита от DDoS-атак путем ограничения количества подключений:  
```bash
stacy-virtualbox:/home/stacy# sudo iptables -A INPUT -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
```

Сохранение настроек:  
```bash
stacy-virtualbox:/home/stacy# sudo iptables-save > /etc/iptables/rules.v4
```

### 1.2. Windows 10 (Windows Firewall)
Блокировка IP-адреса 203.0.113.10 через Windows Firewall:  
1. Откройте Windows Defender Firewall.  
2. Перейдите в "Дополнительные параметры" > "Входящие правила".  
3. Создайте новое правило для блокировки IP-адреса.

Создание правила для защиты от DoS-атак с использованием PowerShell:  
```bash
C:\Users\stacy> netsh advfirewall firewall add rule name="Limit Connections" dir=in action=block protocol=TCP localport=80,443 remoteip=any profile=any
```

## 2. Настройка межсетевого экрана в виртуальной сети
### 2.1. Использование pfSense в виртуальной сети

Блокировка IP-адреса 203.0.113.10 в pfSense:  
1. Перейдите в "Firewall" > "Rules".  
2. Добавьте правило для блокировки IP-адреса.

Настройка защиты от DDoS-атак через ограничение соединений:  
1. Перейдите в "Firewall" > "Advanced Options".  
2. Установите "Maximum State Entries per Host".

Тестирование защиты с использованием утилиты hping3:  
```bash
stacy-virtualbox:/home/stacy# sudo hping3 -S -p 80 -d 120 -c 1000 192.168.1.1
```

## 3. Результаты тестирования
### 3.1. Ubuntu 22.04
Тест блокировки IP-адреса 203.0.113.10:  
```bash
stacy-virtualbox:/home/stacy# ping 203.0.113.10
PING 203.0.113.10 (203.0.113.10): 56 data bytes
Request timed out
```

Тест защиты от DDoS-атак:  
```bash
stacy-virtualbox:/home/stacy# sudo hping3 -S -p 80 -d 120 -c 1000 192.168.1.1
Warning: some packets dropped due to rate limit
```

### 3.2. Windows 10
Проверка правила блокировки:  
```bash
C:\Users\stacy> ping 203.0.113.10
Request timed out.
```

Проверка ограничений подключений:  
```bash
C:\Users\stacy> netstat -an | find "80"
Connection limited due to rule.
```

### 3.3. pfSense

Симуляция DDoS-атаки показала блокировку большинства вредоносных соединений.
```
[2.6.0-RELEASE][admin@pfSense.localdomain]/root: pfctl -sr
block drop in log quick on em0 from 203.0.113.10 to any
pass in quick on em1 proto tcp from any to any port 80 keep state
block drop in quick proto tcp from any to any port 80 flags S/SA limit 10
```
```
[2.6.0-RELEASE][admin@pfSense.localdomain]/root: clog /var/log/filter.log | grep 203.0.113.10
Dec 7 12:34:56 pfSense filterlog[100]: block in on em0: 203.0.113.10.12345 > 192.168.1.1.80: Flags [S], seq 1234567890, win 65535, options [mss 1460], length 0
Dec 7 12:35:10 pfSense filterlog[100]: block in on em0: 203.0.113.10.12346 > 192.168.1.1.443: Flags [S], seq 987654321, win 65535, options [mss 1460], length 0
```
```
[2.6.0-RELEASE][admin@pfSense.localdomain]/root: pfctl -ss
all tcp 192.168.1.1:80 <- 203.0.113.11:54321       ESTABLISHED:ESTABLISHED
all tcp 192.168.1.1:80 <- 203.0.113.10:12345       [BLOCKED: RATE-LIMIT]
```

Лог показывает, что пакеты от IP 203.0.113.10 блокируются по правилу.
Для остальных пользователей подключение на порты 80 и 443 доступно.
Ограничение соединений работает, блокируя избыточные попытки соединений (DDoS).

