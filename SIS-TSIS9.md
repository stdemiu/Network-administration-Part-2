## Анализ методов защиты
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

### Windows 10
Перейдите в "Панель управления", "Система и безопасность", "BitLocker". Выберите диск и включите шифрование.

  ```
  C:\Users\stacy> Get-MpComputerStatus
  AMServiceEnabled    : True
  C:\Users\stacy> netsh advfirewall show allprofiles
  DefaultInboundAction         : Block
  DefaultOutboundAction        : Allow
  ```
  
## Обзор уязвимостей
CVE-2021-3493 — уязвимость в sudo.  

  ```
  stacy-virtualbox:/home/stacy# sudo -u #0 bash
  ```


## Улучшения:

### Ubuntu 22.04
Регулярно обновляйте систему с помощью `apt`. Включите и настройте брандмауэр ufw.

### Windows 10
Включите BitLocker для защиты данных. Обновляйте систему через "Центр обновлений".
