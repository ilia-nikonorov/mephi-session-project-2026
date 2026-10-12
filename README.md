# Сессионный проект по курсу «Безопасность GNU/Linux»

Настройка базовых средств защиты ОС GNU/Linux (`mephi-2026.domain.local`, студент `М265919`).

## Описание выполнения задания

| Раздел | Задание | Как выполнено | Файл с результатом |
| :--- | :--- | :--- | :--- |
| **1. Установка дистрибутива** | 1.1. Настройка сети | Настроено получение IP-адреса по DHCP на сетевом интерфейсе | `history.out` |
| | 1.2. Настройка имени хоста | `hostnamectl set-hostname mephi-2026.domain.local` | `history.out`, `journalctl.out` |
| | 1.3. Проверка сетевой связности | `ping -c 4 8.8.8.8 > ~/ping.out` | `ping.out` |
| **2. Управление ПО** | 2.1. Обновление дистрибутива | `dnf update -y` | `dnf.out` |
| | 2.2. Установка пакетов из репозиториев | `dnf install -y nginx libcap-ng-utils` | `dnf.out` |
| | 2.3. Установка локального RPM-пакета | `dnf download --destdir=/tmp tcpdump` и `rpm -ivh /tmp/tcpdump*.rpm` | `dnf.out`, `history.out` |
| **3. Управление ФС** | 3.1. Создание файловой системы | Создан раздел на втором диске (`fdisk`), отформатирован в `ext4` с меткой `MEPHI_WEB` (`mkfs.ext4 -L MEPHI_WEB`) | `history.out`, `fstab` |
| | 3.2. Монтирование файловой системы | Создана `/mephi-web`, добавлена запись с `LABEL=MEPHI_WEB` в `/etc/fstab`, выполнено `mount -a` | `fstab`, `stat.out` |
| **4. Управление сервисами** | 4.1. Управление веб-сервером | `systemctl start nginx` и `systemctl enable nginx` | `journalctl.out` |
| | 4.2. Журналирование | `journalctl -u nginx -b > ~/journalctl.out` | `journalctl.out` |
| **5. Управление доступом** | 5.1. Дискреционное управление доступом (DAC) | Созданы пользователи `user1` (5501), `user2` (5502), `user3` (5503) в группе `developers` и `curator1`, `curator2` в группе `curators` (4444). На `/data/mephi-2026` выставлены права `2770` и ACL (`setfacl -m` / `setfacl -d -m`) | `stat.out`, `passwd`, `group` |
| | 5.2. Привилегии | Снят SUID-бит с `/usr/sbin/tcpdump`, выданы привилегии `setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump`, проверен запуск от `user1` | `getcap.out`, `history.out` |
| | 5.3. Мандатное управление доступом (MAC) | Проверен режим SELinux (`Enforcing`), в `/etc/nginx/nginx.conf` указан корень `/mephi-web`, задан контекст `httpd_sys_content_t` через `semanage fcontext` и `restorecon` | `getenforce.out`, `stat.out` |
| **6. Аутентификация** | 6.1. Ограничение входа | Кураторам ограничен вход (`/sbin/nologin` в `/etc/passwd`) | `passwd`, `history.out` |
| | 6.2. Управление паролями | Установлен `minlen = 12` в `/etc/security/pwquality.conf`, срок действия пароля 90 дней в `/etc/login.defs` и через `chage -M 90` | `shadow`, `pwquality.conf` |
| **7. Тестирование** | 7.1. Создание web-страницы | В `/mephi-web/index.html` записана строка `Hello from Student: М265919` | `curl.out` |
| | 7.2. Проверка доступности | Проверено через `curl http://localhost/` | `curl.out`, `mephi-screenshot.png` |
| **8. Публикация** | 8.1–8.4. Артефакты проекта | Все выходные файлы и конфиги из `/etc` загружены в репозиторий | Все файлы |
