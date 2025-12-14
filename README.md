# Multiplayer BlackJack - Blazor WebAssembly Application

## 📖 Описание проекта (RU) / Project Description (EN)

### 🇷🇺 Русский

**Multiplayer BlackJack** - это полнофункциональная веб-реализация классической карточной игры Блэкджек с поддержкой нескольких игроков в режиме реального времени. Проект демонстрирует современную архитектуру веб-приложений с использованием .NET 8, Blazor WebAssembly и SignalR для обеспечения мгновенного взаимодействия между игроками.

#### Суть проекта

Это многопользовательская онлайн-игра в Блэкджек, где:
- Несколько игроков могут одновременно подключаться к игровой сессии
- Игроки соревнуются с дилером, пытаясь набрать 21 очко или как можно ближе к этому значению
- Все действия синхронизируются в реальном времени между всеми участниками
- Игровое состояние сохраняется в базе данных PostgreSQL

#### Основные возможности

- ✅ **Создание игровых комнат** - игроки могут создавать собственные игровые сессии
- ✅ **Присоединение к играм** - подключение к существующим игровым комнатам
- ✅ **Игровой процесс в реальном времени** - мгновенная синхронизация действий всех игроков
- ✅ **Классические правила Блэкджека** - действия "Hit" (взять карту) и "Stand" (остановиться)
- ✅ **Управление игровой сессией** - отслеживание состояния каждого игрока
- ✅ **Визуализация карт** - графическое отображение карт игроков и дилера

---

### 🇬🇧 English

**Multiplayer BlackJack** is a full-featured web implementation of the classic Blackjack card game with real-time multiplayer support. The project demonstrates modern web application architecture using .NET 8, Blazor WebAssembly, and SignalR for instant player interaction.

#### Project Essence

This is a multiplayer online Blackjack game where:
- Multiple players can simultaneously connect to a game session
- Players compete against the dealer, trying to reach 21 points or as close as possible
- All actions are synchronized in real-time between all participants
- Game state is persisted in PostgreSQL database

#### Key Features

- ✅ **Game Room Creation** - players can create their own game sessions
- ✅ **Join Games** - connect to existing game rooms
- ✅ **Real-time Gameplay** - instant synchronization of all player actions
- ✅ **Classic Blackjack Rules** - "Hit" (take a card) and "Stand" (stop) actions
- ✅ **Game Session Management** - tracking the state of each player
- ✅ **Card Visualization** - graphical display of player and dealer cards

---

## 🏗️ Архитектура / Architecture

### Технологический стек / Technology Stack

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

### Структура проекта / Project Structure

```
Multiplayer-Blackjack-Blazor/
├── BlackJack.API/              # Backend Web API
│   ├── Controllers/            # REST API controllers
│   ├── Hubs/                   # SignalR hubs (GameHub)
│   ├── Services/               # Business logic services
│   ├── Data/                   # Database context
│   └── Migrations/             # EF Core migrations
│
├── BlackJack.BlazorWasm/       # Frontend Blazor WebAssembly
│   ├── Pages/                  # Razor pages (GameRoom, GameMenu)
│   ├── Services/               # Client services
│   ├── Layout/                 # Layout components
│   └── wwwroot/                # Static files (card images)
│
└── BlackJack.Domain/           # Shared domain layer
    ├── Entities/               # Domain entities (Player, Dealer, Card, Deck, GameSession)
    └── Models/                 # Data transfer objects
```

### Основные компоненты / Key Components

#### Entities (Сущности)
- **GameSession** - Игровая сессия с участниками и состоянием игры
- **Player** - Игрок с картами и счетом
- **Dealer** - Дилер, раздающий карты
- **Card** - Карта с мастью и значением
- **Deck** - Колода карт

#### SignalR Hub Methods
- `CreateGame` - Создание новой игры
- `JoinGame` - Присоединение к игре
- `LeaveGame` - Выход из игры
- `StartGame` - Начало игры
- `PlayerAction` - Действия игрока (Hit/Stand)

---

## 🚀 Как запустить / How to Run

### Предварительные требования / Prerequisites

- .NET 8.0 SDK or later
- PostgreSQL 12+ database
- Visual Studio 2022 or VS Code (recommended)

### Шаги установки / Installation Steps

1. **Клонировать репозиторий / Clone the repository**
   ```bash
   git clone https://github.com/Aphelack/Multiplayer-Blackjack-Blazor.git
   cd Multiplayer-Blackjack-Blazor
   ```

2. **Настроить базу данных / Configure Database**
   
   Создайте PostgreSQL базу данных и обновите строку подключения в `BlackJack.API/appsettings.json`:
   ```json
   "ConnectionStrings": {
     "DefaultConnection": "Host=localhost;Database=blackjack_db;Username=your_user;Password=your_password"
   }
   ```

3. **Применить миграции / Apply Migrations**
   ```bash
   cd BlackJack.API
   dotnet ef database update
   ```

4. **Запустить Backend / Run Backend**
   ```bash
   cd BlackJack.API
   dotnet run
   ```
   API будет доступен на https://localhost:7113

5. **Запустить Frontend / Run Frontend**
   
   В отдельном терминале:
   ```bash
   cd BlackJack.BlazorWasm
   dotnet run
   ```
   Приложение откроется в браузере

---

## 🎮 Как играть / How to Play

1. **Создать игру** - Укажите название игры и создайте новую игровую комнату
2. **Пригласить игроков** - Поделитесь идентификатором игры с друзьями
3. **Присоединиться** - Другие игроки подключаются по ID игры
4. **Начать игру** - Когда все готовы (минимум 2 игрока)
5. **Играть** - Используйте кнопки "Hit" (взять карту) или "Stand" (остановиться)
6. **Победа** - Игрок, набравший ближайшее к 21 очко без перебора, побеждает

---

## 🔧 Технические детали / Technical Details

### SignalR Real-time Communication
Проект использует SignalR для обеспечения двусторонней связи в реальном времени между клиентами и сервером, что позволяет мгновенно синхронизировать игровое состояние между всеми участниками.

### Entity Framework Core
Используется для управления данными игровых сессий, игроков и их состояний с автоматической генерацией схемы базы данных через миграции.

### Blazor WebAssembly
Позволяет запускать C# код непосредственно в браузере через WebAssembly, обеспечивая производительность, близкую к нативным приложениям.

---

## 📝 Лицензия / License

Этот проект создан в образовательных целях / This project is created for educational purposes.

---

## 👨‍💻 Автор / Author

[Aphelack](https://github.com/Aphelack) 
