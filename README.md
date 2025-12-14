# Multiplayer BlackJack - Blazor WebAssembly Application

## 📖 Описание проекта

**Multiplayer BlackJack** - это полнофункциональная веб-реализация классической карточной игры Блэкджек с поддержкой нескольких игроков в режиме реального времени. Проект демонстрирует современную архитектуру веб-приложений с использованием .NET 8, Blazor WebAssembly и SignalR для обеспечения мгновенного взаимодействия между игроками.

### Суть проекта

Это многопользовательская онлайн-игра в Блэкджек, где:
- Несколько игроков могут одновременно подключаться к игровой сессии
- Игроки соревнуются с дилером, пытаясь набрать 21 очко или как можно ближе к этому значению
- Все действия синхронизируются в реальном времени между всеми участниками
- Игровое состояние сохраняется в базе данных PostgreSQL

### Основные возможности

- ✅ **Создание игровых комнат** - игроки могут создавать собственные игровые сессии
- ✅ **Присоединение к играм** - подключение к существующим игровым комнатам
- ✅ **Игровой процесс в реальном времени** - мгновенная синхронизация действий всех игроков
- ✅ **Классические правила Блэкджека** - действия "Hit" (взять карту) и "Stand" (остановиться)
- ✅ **Управление игровой сессией** - отслеживание состояния каждого игрока
- ✅ **Визуализация карт** - графическое отображение карт игроков и дилера

---

## 🏗️ Архитектура

### Технологический стек

#### Backend
- **ASP.NET Core 8.0** - REST API framework
- **SignalR** - Real-time bidirectional communication
- **Entity Framework Core 9.0** - ORM for database operations
- **PostgreSQL** - Relational database for game state persistence
- **C# 12** - Programming language

#### Frontend
- **Blazor WebAssembly 8.0** - Client-side SPA framework
- **SignalR Client** - Real-time communication with backend
- **C#** - Frontend programming language (via WebAssembly)

#### Domain Layer
- **Shared Entities** - Common models used by both frontend and backend

### Структура проекта

```
Multiplayer-Blackjack-Blazor/
├── BlackJack.API/              # Backend Web API
│   ├── Controllers/            # REST API контроллеры
│   ├── Hubs/                   # SignalR хабы (GameHub)
│   ├── Services/               # Сервисы бизнес-логики
│   ├── Data/                   # Контекст базы данных
│   └── Migrations/             # EF Core миграции
│
├── BlackJack.BlazorWasm/       # Frontend Blazor WebAssembly
│   ├── Pages/                  # Razor страницы (GameRoom, GameMenu)
│   ├── Services/               # Клиентские сервисы
│   ├── Layout/                 # Компоненты макета
│   └── wwwroot/                # Статические файлы (изображения карт)
│
└── BlackJack.Domain/           # Общий доменный слой
    ├── Entities/               # Доменные сущности (Player, Dealer, Card, Deck, GameSession)
    └── Models/                 # Объекты передачи данных
```

### Основные компоненты

#### Сущности
- **GameSession** - Игровая сессия с участниками и состоянием игры
- **Player** - Игрок с картами и счетом
- **Dealer** - Дилер, раздающий карты
- **Card** - Карта с мастью и значением
- **Deck** - Колода карт

#### SignalR Hub методы
- `CreateGame` - Создание новой игры
- `JoinGame` - Присоединение к игре
- `LeaveGame` - Выход из игры
- `StartGame` - Начало игры
- `PlayerAction` - Действия игрока (Hit/Stand)

---

## 🚀 Развертывание на Linux

### Предварительные требования

- .NET 8.0 SDK или выше
- PostgreSQL 12+ база данных
- Git

### Установка на Ubuntu

#### 1. Установка .NET 8 SDK

```bash
# Добавление репозитория Microsoft
wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Обновление списка пакетов
sudo apt update

# Установка .NET SDK 8.0
sudo apt install -y dotnet-sdk-8.0

# Проверка установки
dotnet --version
```

#### 2. Установка PostgreSQL

```bash
# Установка PostgreSQL
sudo apt install -y postgresql postgresql-contrib

# Запуск PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Создание базы данных и пользователя
sudo -u postgres psql << EOF
CREATE DATABASE blackjack_db;
CREATE USER blackjack_user WITH ENCRYPTED PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE blackjack_db TO blackjack_user;
\q
EOF
```

#### 3. Клонирование и настройка проекта

```bash
# Клонирование репозитория
git clone https://github.com/ZINA312/Multiplayer-Blackjack-Blazor.git
cd Multiplayer-Blackjack-Blazor

# Настройка строки подключения к БД
nano BlackJack.API/appsettings.json
```

Обновите строку подключения:
```json
"ConnectionStrings": {
  "DefaultConnection": "Host=localhost;Database=blackjack_db;Username=blackjack_user;Password=your_secure_password"
}
```

#### 4. Применение миграций и запуск

```bash
# Восстановление зависимостей
dotnet restore

# Применение миграций базы данных
cd BlackJack.API
dotnet ef database update
cd ..

# Запуск Backend (в отдельном терминале)
cd BlackJack.API
dotnet run --urls "http://localhost:5052;https://localhost:7052"

# Запуск Frontend (в другом терминале)
cd BlackJack.BlazorWasm
dotnet run
```

---

### Установка на Arch Linux

#### 1. Установка .NET 8 SDK

```bash
# Установка .NET SDK из официальных репозиториев
sudo pacman -Syu
sudo pacman -S dotnet-sdk

# Проверка установки
dotnet --version
```

#### 2. Установка PostgreSQL

```bash
# Установка PostgreSQL
sudo pacman -S postgresql

# Инициализация кластера базы данных
sudo -u postgres initdb -D /var/lib/postgres/data

# Запуск и автозапуск PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Создание базы данных и пользователя
sudo -u postgres psql << EOF
CREATE DATABASE blackjack_db;
CREATE USER blackjack_user WITH ENCRYPTED PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE blackjack_db TO blackjack_user;
\q
EOF
```

#### 3. Клонирование и настройка проекта

```bash
# Установка Git (если еще не установлен)
sudo pacman -S git

# Клонирование репозитория
git clone https://github.com/ZINA312/Multiplayer-Blackjack-Blazor.git
cd Multiplayer-Blackjack-Blazor

# Настройка строки подключения к БД
nano BlackJack.API/appsettings.json
```

Обновите строку подключения:
```json
"ConnectionStrings": {
  "DefaultConnection": "Host=localhost;Database=blackjack_db;Username=blackjack_user;Password=your_secure_password"
}
```

#### 4. Применение миграций и запуск

```bash
# Восстановление зависимостей
dotnet restore

# Установка Entity Framework инструментов (если требуется)
dotnet tool install --global dotnet-ef

# Применение миграций базы данных
cd BlackJack.API
dotnet ef database update
cd ..

# Запуск Backend (в отдельном терминале)
cd BlackJack.API
dotnet run --urls "http://localhost:5052;https://localhost:7052"

# Запуск Frontend (в другом терминале)
cd BlackJack.BlazorWasm
dotnet run
```

---

### Общие рекомендации для Linux

#### Настройка файрвола (опционально)

Если используете UFW (Ubuntu):
```bash
sudo ufw allow 5052/tcp
sudo ufw allow 7052/tcp
sudo ufw allow 5432/tcp  # PostgreSQL
```

Если используете firewalld (некоторые дистрибутивы):
```bash
sudo firewall-cmd --permanent --add-port=5052/tcp
sudo firewall-cmd --permanent --add-port=7052/tcp
sudo firewall-cmd --permanent --add-port=5432/tcp
sudo firewall-cmd --reload
```

#### Запуск как системный сервис (Production)

Создайте файл systemd service для Backend:

```bash
sudo nano /etc/systemd/system/blackjack-api.service
```

Содержимое файла:
```ini
[Unit]
Description=BlackJack API Service
After=network.target postgresql.service

[Service]
Type=notify
User=your_username
WorkingDirectory=/path/to/Multiplayer-Blackjack-Blazor/BlackJack.API
ExecStart=/usr/bin/dotnet run --urls "http://0.0.0.0:5052;https://0.0.0.0:7052"
Restart=on-failure
RestartSec=10
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target
```

Активация сервиса:
```bash
sudo systemctl daemon-reload
sudo systemctl enable blackjack-api
sudo systemctl start blackjack-api
sudo systemctl status blackjack-api
```

#### Использование Nginx как обратного прокси (опционально)

Установка Nginx:
```bash
# Ubuntu
sudo apt install nginx

# Arch Linux
sudo pacman -S nginx
```

Настройка:
```bash
sudo nano /etc/nginx/sites-available/blackjack
```

Содержимое конфигурации:
```nginx
server {
    listen 80;
    server_name your_domain.com;

    location / {
        proxy_pass http://localhost:5052;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Активация:
```bash
# Ubuntu
sudo ln -s /etc/nginx/sites-available/blackjack /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

# Arch Linux
sudo nginx -t
sudo systemctl restart nginx
```

---

## 🎮 Как играть

1. **Создать игру** - Укажите название игры и создайте новую игровую комнату
2. **Пригласить игроков** - Поделитесь идентификатором игры с друзьями
3. **Присоединиться** - Другие игроки подключаются по ID игры
4. **Начать игру** - Когда все готовы (минимум 2 игрока)
5. **Играть** - Используйте кнопки "Hit" (взять карту) или "Stand" (остановиться)
6. **Победа** - Игрок, набравший ближайшее к 21 очко без перебора, побеждает

---

## 🔧 Технические детали

### SignalR Real-time Communication
Проект использует SignalR для обеспечения двусторонней связи в реальном времени между клиентами и сервером, что позволяет мгновенно синхронизировать игровое состояние между всеми участниками.

### Entity Framework Core
Используется для управления данными игровых сессий, игроков и их состояний с автоматической генерацией схемы базы данных через миграции.

### Blazor WebAssembly
Позволяет запускать C# код непосредственно в браузере через WebAssembly, обеспечивая производительность, близкую к нативным приложениям.

---

## 📝 Лицензия

Этот проект создан в образовательных целях.

---

## 👨‍💻 Автор

[Aphelack](https://github.com/Aphelack) 
