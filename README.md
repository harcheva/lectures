# Задание учебной практики  УП.04 Сопровождение и обслуживание программного обеспечения компьютерных систем

<div align="center">
  <h1>📊 Учебная практика УП.04</h1>
  <h3>«Сопровождение и обслуживание ПО компьютерных систем»</h3>
  <p>Настройка Zabbix 6.4, мониторинг Windows и Linux</p>
</div>

---

## 📌 Содержание
1. [День 1: Настройка сервера Zabbix и MySQL](#-день-1-настройка-сервера-zabbix-и-mysql)
2. [День 2: Агентный мониторинг Windows](#-день-2-агентный-мониторинг-windows)
3. [День 3: Агентный мониторинг Linux](#-день-3-агентный-мониторинг-linux)
   
---

## 🖥️ День 1: Настройка сервера Zabbix и MySQL
### 🔹 Шаг 1: Установка РЕД ОС и обновление системы
```bash
# Переход в сеанс root
su -

# Настройка политик SELinux
setsebool -P httpd_can_network_connect on
setsebool -P httpd_can_network_connect_db on

# Обновление системы
dnf update

# Установка необходимых пакетов
dnf install httpd zabbix-apache-conf zabbix-sql-scripts
systemctl enable httpd
###🔹 Шаг 2: Установка MySQL 8.4
bash
# Скачивание и установка репозитория MySQL
wget https://dev.mysql.com/get/mysql84-community-release-el8-1.noarch.rpm
sudo rpm -ivh mysql84-community-release-el8-1.noarch.rpm

# Установка MySQL Server
sudo dnf install mysql-community-server

# Запуск и настройка MySQL
sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo mysql_secure_installation
###🔹 Шаг 3: Установка Zabbix Server
bash
# Добавление репозитория Zabbix
sudo rpm -Uvh https://repo.zabbix.com/zabbix/6.4/rhel/8/x86_64/zabbix-release-6.4-1.el8.noarch.rpm

# Установка компонентов Zabbix
sudo dnf install zabbix-server-mysql zabbix-web-mysql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent

# Импорт схемы БД
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
###🔹 Шаг 4: Настройка веб-интерфейса
Откройте в браузере: http://<IP_сервера>/zabbix

Следуйте инструкциям мастера установки.

Используйте данные БД:

Имя пользователя: zabbix

Пароль: Root_123

## 🐧День 2: Агентный мониторинг Windows
###🔹 Установка Zabbix Agent
Скачайте агент для Windows:
Zabbix Agent 6.4.21

Распакуйте в C:\Program Files\zabbix.

Настройте zabbix_agentd.conf:

ini
Server=192.168.88.208
Hostname=host.windows.home
ListenPort=10050
###🔹 Настройка в Zabbix
Создайте узел сети host.windows.home.

Примените шаблон Agent Windows Monitor.

Добавьте элементы данных из таблицы:

Элемент данных	Ключ	Описание
Доступность по ICMP	icmpping[,3,,,]	Проверка связи
Загрузка CPU	system.cpu.util[,system]	Утилизация процессора
###🔹 Низкоуровневое обнаружение
Сетевые интерфейсы:
Используйте LLD-правило net.if.discovery с фильтром @win-eth.

Дисковое пространство:
Правило vfs.fs.discovery для мониторинга разделов.

##🐧 День 3: Агентный мониторинг Linux
###🔹 Установка Zabbix Agent

sudo apt-get install zabbix-agent
sudo nano /etc/zabbix/zabbix_agentd.conf

AllowRoot=1
Server=<IP_Zabbix_Server>
Hostname=host.linux.home
###🔹 Настройка в Zabbix
Создайте шаблон Agent Linux Monitor.

Добавьте элементы данных из таблицы:

Элемент данных	Ключ	Описание
Информация об ОС	system.uname	Версия ядра
Свободная память	vm.memory.size[free]	Объем свободной памяти
###🔹 Низкоуровневое обнаружение
Диски:

vfs.fs.size[{#FSNAME},used]
vfs.fs.size[{#FSNAME},total]
Фильтр: Регулярное выражение [a-z] для исключения подкаталогов.


<div align="center"> <p>🔹 <em>Материалы подготовлены для учебной практики УП.04</em> 🔹</p> <p>📅 <strong>Дата последнего обновления:</strong> 2025-05-09</p> </div> ```

 

   
