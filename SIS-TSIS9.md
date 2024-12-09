## 1. Анализ методов защиты
- AppArmor используется для контроля доступа.  
  ```
  stacy-virtualbox:/home/stacy# sudo aa-status
  status: apparmor is enabled
  ```

  Проверка состояния:
  ```
  stacy-virtualbox:/home/stacy# sudo ufw status
  Status: inactive
  stacy-virtualbox:/home/stacy# sudo apt update && sudo apt upgrade
  ```

### 1.2. Windows 10
Перейдите в "Панель управления", "Система и безопасность", "BitLocker". Выберите диск и включите шифрование.

  ```
  C:\Users\stacy> Get-MpComputerStatus
  AMServiceEnabled    : True
  C:\Users\stacy> netsh advfirewall show allprofiles
  DefaultInboundAction         : Block
  DefaultOutboundAction        : Allow
  ```
  
## 2. Обзор уязвимостей
CVE-2021-3493 — уязвимость в sudo.  

  ```
  stacy-virtualbox:/home/stacy# sudo -u #0 bash
  ```


## 4. Улучшения:

### 4.1. Ubuntu 22.04
Регулярно обновляйте систему с помощью `apt`. Включите и настройте брандмауэр ufw.

### 4.2. Windows 10
Включите BitLocker для защиты данных. Обновляйте систему через "Центр обновлений".
