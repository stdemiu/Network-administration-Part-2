# 11.	Управление обновлениями и патчами:
СРО: Настройте автоматическое управление обновлениями и патчами на сервере с помощью утилит (например, yum для CentOS или apt для Ubuntu). Представьте отчет о процессе обновления с анализом влияния на безопасность системы.
СРОП: Разработайте план управления обновлениями для крупной сети, включающий различные операционные системы и ПО. Оцените риски, связанные с отсутствием обновлений, и представьте отчет с рекомендациями по автоматизации процессов.

## 1. Настройка автоматического управления обновлениями
### 1.1. Ubuntu 22.04 (apt)
Настройка автоматических обновлений через `unattended-upgrades`:  
1. Установите пакет:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo apt install unattended-upgrades
   ```
2. Включите автоматические обновления:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo dpkg-reconfigure --priority=low unattended-upgrades
   ```
3. Проверьте файл конфигурации:  
   ```bash
   stacy-virtualbox:/home/stacy# cat /etc/apt/apt.conf.d/50unattended-upgrades
   ```
4. Запустите ручное обновление для проверки:  
   ```bash
   stacy-virtualbox:/home/stacy# sudo unattended-upgrades --dry-run
   ```

### 1.2. Windows 10
Настройка автоматических обновлений через Windows Update:  
1. Откройте "Центр обновления Windows".  
2. Выберите "Параметры" > "Автоматическая установка обновлений".  
3. Для проверки обновлений выполните команду в PowerShell:  
   ```bash
   C:\Users\stacy> wuauclt /detectnow
   ```

## 2. План управления обновлениями для крупной сети
### 2.1. Общие принципы
1. Используйте централизованный инструмент управления, например:
   WSUS для Windows-систем.  
   - Ansible или Chef для Linux-систем.  

2. Разделите оборудование на группы по уровню критичности:
   - Критическая инфраструктура (обновляется вне зависимости от расписания).  
   - Пользовательские системы (обновления раз в неделю).  
   - Тестовые машины (тестирование патчей перед развёртыванием).

### 2.2. Оценка рисков
Риски отсутствия обновлений:  
- Уязвимость к известным эксплойтам.  
- Нарушение совместимости с современным ПО.  
- Утечка данных из-за уязвимостей.  

Риски некорректных обновлений:  
- Снижение производительности.  
- Поломка зависимостей.

### 2.3. Улучшения по автоматизации

- Используйте инструменты мониторинга для оценки уязвимостей (например, Nessus или OpenVAS).  
- Включите автоматические обновления для критических патчей.  
- Создавайте резервные копии перед установкой обновлений.

## 3. Результаты тестирования
### 3.1. Ubuntu 22.04
Проверка автоматических обновлений:  
```bash
stacy-virtualbox:/home/stacy# sudo unattended-upgrades --dry-run
Checking: install package security updates
```

Проверка установленных обновлений:  
```bash
stacy-virtualbox:/home/stacy# cat /var/log/unattended-upgrades/unattended-upgrades.log
2024-12-07 12:30:00 Installed: libssl3, openssl
```

### 3.2. Windows 10
Проверка доступных обновлений через PowerShell:  
```bash
C:\Users\stacy> Get-WindowsUpdateLog
Update KB5023792 installed successfully
```

Проверка автоматических настроек:  
```bash
C:\Users\stacy> gpresult /H report.html
```