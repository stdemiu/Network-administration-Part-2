# 12.	Шифрование и защита данных:
СРО: Настройте систему шифрования данных на уровне файловой системы с использованием LUKS (для Linux) или BitLocker (для Windows). Подготовьте отчет о настройках шифрования, его использовании и защите ключей.
СРОП: Исследуйте и реализуйте шифрование данных при передаче через сеть (например, с использованием OpenVPN или TLS). Представьте результаты тестирования эффективности шифрования.

## 1. Настройка шифрования данных на уровне файловой системы
### 1.1. Ubuntu 22.04 (LUKS)
Создание зашифрованного раздела с использованием LUKS:  
1. Инициализация LUKS на разделе `/dev/sdb1`:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo cryptsetup luksFormat /dev/sdb1
   ```
2. Открытие зашифрованного раздела:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo cryptsetup open /dev/sdb1 encrypted_partition
   ```
3. Создание файловой системы:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo mkfs.ext4 /dev/mapper/encrypted_partition
   ```
4. Монтирование раздела:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo mount /dev/mapper/encrypted_partition /mnt
   ```

### 1.2. Windows 10 (BitLocker)
Включение шифрования BitLocker:  
1. Откройте "Панель управления" > "Система и безопасность" > "Шифрование диска BitLocker".  
2. Включите BitLocker для выбранного диска.  
3. Сохраните ключ восстановления на внешний носитель или в учетную запись Microsoft.  

Проверка состояния BitLocker:  
```bash
C:\Users\stacy> manage-bde -status
Volume C:
    Conversion Status: Fully Encrypted
    Encryption Method: AES 128-bit with Diffuser
```

## 2. Шифрование данных при передаче через сеть
### 2.1. Использование OpenVPN
Настройка OpenVPN-сервера на Ubuntu:  
1. Установите OpenVPN:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo apt install openvpn easy-rsa
   ```
2. Сгенерируйте ключи и сертификаты:  
   ```bash
   stacy-virtualbox:/home/stacy# ./easyrsa init-pki
   stacy-virtualbox:/home/stacy# ./easyrsa build-ca nopass
   ```
3. Настройте серверный конфигурационный файл `/etc/openvpn/server.conf`.  

Проверка работы OpenVPN:  
```bash
stacy-virtualbox:/home/stacy# sudo systemctl status openvpn@server
● openvpn@server.service - OpenVPN connection to server
   Active: active (running)
```

### 2.2. Использование TLS

Настройка TLS для веб-сервера Apache:  
1. Установите сертификат через Let’s Encrypt:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo apt install certbot python3-certbot-apache
   stacy-virtualbox:/home/stacy# sudo certbot --apache -d stacydomain.com
   ```
2. Проверьте, что HTTPS работает:  
   ```bash
   stacy-virtualbox:/home/stacy# curl -I https://stacydomain.com
   HTTP/2 200
   ```

## 3. Результаты тестирования
### 3.1. Ubuntu 22.04
Проверка монтирования зашифрованного раздела:  
```bash
stacy-virtualbox:/home/stacy# lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sdb               8:16   0   100G  0 disk  
└─encrypted_partition
                 254:0    0   100G  0 crypt /mnt
```

Проверка работы OpenVPN:  
```bash
stacy-virtualbox:/home/stacy# openvpn --version
OpenVPN 2.5.1 x86_64-pc-linux-gnu
```

### 3.2. Windows 10
Проверка шифрования BitLocker:  
```bash
C:\Users\stacy> manage-bde -status
Volume C:
    Encryption Status: Fully Encrypted
    Protection Status: On
```

## 4. Улучшения

### 4.1. Ubuntu 22.04
- Регулярно обновляйте ключи шифрования LUKS.  
- Храните резервные копии ключей в безопасном месте.  

### 4.2. Windows 10
- Регулярно проверяйте состояние шифрования с помощью `manage-bde`.  
- Храните ключ восстановления в защищенном месте.  

### 4.3. Шифрование при передаче данных
- Используйте OpenVPN или TLS для защиты соединений.  
- Регулярно обновляйте сертификаты TLS.  
