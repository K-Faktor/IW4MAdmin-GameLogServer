# 🎮 IW4MAdmin Game Log Server

Сервер игровых логов для IW4MAdmin. Минимальная установка на Debian 13 (trixie) без лишних зависимостей.

---

## 📦 Требования

- **ОС:** Debian 13 (trixie) или совместимый дистрибутив
- **Python:** 3.11+
- **Права:** `sudo` для установки в `/opt`

---

## 🚀 Установка

### 1️⃣ Установка ядра Python 3

Устанавливаем только необходимое — `pip` уже входит в состав `venv`:

```bash
sudo apt update
sudo apt install python3 python3-venv -y
```

> 💡 Пакет `python3-pip` **не нужен** — `pip` доступен внутри виртуального окружения.

---

### 2️⃣ Загрузка проекта без Git

```bash
cd /tmp
curl -L https://github.com/K-Faktor/IW4MAdmin-GameLogServer/archive/refs/heads/master.tar.gz -o gamelog.tar.gz
tar -xzf gamelog.tar.gz
sudo mv IW4MAdmin-GameLogServer-master /opt/gamelogserver
cd /opt/gamelogserver
```

---

### 3️⃣ Создание виртуального окружения

```bash
python3 -m venv venv
./venv/bin/pip install flask flask_restful requests
```

---

### 4️⃣ Запуск сервера

Запуск напрямую — **без активации** виртуального окружения:

```bash
./venv/bin/python runserver.py
```

---

### 5️⃣ Автозапуск через systemd *(опционально)*

Создайте unit-файл:

```bash
sudo nano /etc/systemd/system/gamelogserver.service
```

Содержимое:

```ini
[Unit]
Description=Game Log Server
After=network.target

[Service]
Type=simple
WorkingDirectory=/opt/gamelogserver
ExecStart=/opt/gamelogserver/venv/bin/python runserver.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Активируйте сервис:

```bash
sudo systemctl daemon-reload
sudo systemctl start gamelogserver
sudo systemctl enable gamelogserver
```

---

## 🛠 Управление сервисом

| Команда | Описание |
|---|---|
| `sudo systemctl status gamelogserver` | Проверить статус |
| `sudo systemctl restart gamelogserver` | Перезапустить |
| `sudo systemctl stop gamelogserver` | Остановить |
| `sudo journalctl -u gamelogserver -f` | Смотреть логи в реальном времени |

---

## 📂 Структура

```
/opt/gamelogserver/
├── venv/              # Виртуальное окружение Python
├── runserver.py       # Точка входа
└── ...                # Исходный код проекта
```

## 🛠 Настройка IW4MAdmin
* Обновите файл `IW4MAdminSettings.json` изменив значение `GameLogServerUrl` на "http://<remote_server_ip>:1625"
* Пример &mdash; `"GameLogServerUrl": "http://192.168.1.123:1625",`

---

## 📝 Лицензия

См. оригинальный репозиторий: [IW4MAdmin-GameLogServer](https://github.com/RaidMax/IW4MAdmin-GameLogServer)