Домашнее задание к занятию «Система мониторинга Zabbix» - `Avvakumov Oleg`

---

### Задание 1

'Установите Zabbix Server с веб-интерфейсом.
Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.'


1. `$ sudo -s`
2. `wget https://repo.zabbix.com/zabbix/7.0/ubuntu-arm64/pool/main/z/zabbix-release/zabbix- release_latest_7.0+ubuntu24.04_all.deb`
3. `dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb`
4. `apt update`
5. `apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent`
6. `sudo -u postgres createuser --pwprompt zabbix`
7. `sudo -u postgres createdb -O zabbix zabbix`
8. `zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix`
9. `DBPassword=********   #password hidden`
10. `systemctl restart zabbix-server zabbix-agent apache2`11. `systemctl enable zabbix-server zabbix-agent apache2`


![1](1.png)`
![2](2.png)`
![3](3.png)`

---

### Задание 2

'Установите Zabbix Agent на два хоста.
Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.'

```
1. Добавляем репозиторий Zabbix 7.0 для Ubuntu
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix- # release_7.0-2+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_7.0-2+ubuntu24.04_all.deb
sudo apt update

2. Устанавливаем Zabbix Agent
sudo apt install -y zabbix-agent

3. 3. Проверяем, что установился
dpkg -l | grep zabbix-agent

# Меняем Server (IP Zabbix Server)
sudo sed -i 's/^Server=127.0.0.1/Server=192.168.31.246/' /etc/zabbix/zabbix_agentd.conf

# Меняем ServerActive
sudo sed -i 's/^ServerActive=127.0.0.1/ServerActive=192.168.31.246/' /etc/zabbix/zabbix_agentd.conf

# Меняем Hostname
sudo sed -i 's/^Hostname=Zabbix server/Hostname=Linux Mint/' /etc/zabbix/zabbix_agentd.conf

# Проверяем изменения
sudo grep -E "^Server=|^ServerActive=|^Hostname=" /etc/zabbix/zabbix_agentd.conf

# Запускаем и включаем автозапуск
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent

# Проверяем статус
sudo systemctl status zabbix-agent

```
![1](4.png)
![2](5.png)
![3](6.png)
![4](7.png)
![5](8.png)
![6](9.png)
![7](10.png)
![8](11.png)
![9](12.png)
![10](13.png)
![11](14.png)
---


