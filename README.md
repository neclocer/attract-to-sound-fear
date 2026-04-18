# AttractToSound — Fear & Voice Integration 

> Minecraft Forge мод для версии 1.20.1
> Форк оригинального AttractToSound с добавлением системы страха и интеграции с Simple Voice Chat
> NECLOCER DEV · 2025

---

## Стек

- **Язык:** Java 17
- **Платформа:** Minecraft Forge 1.20.1
- **Сборка:** Gradle + ForgeGradle
- **Интеграция:** Simple Voice Chat API

---

## Что делает мод

Оригинальный мод **AttractToSound** заставляет мобов реагировать на звуки в игре.

В этом форке добавлена **система страха** и **интеграция с войс-чатом**:

- Чем больше страх игрока — тем агрессивнее реагируют мобы
- Голос игрока в войс-чате повышает уровень страха
- Разные типы мобов реагируют по-разному

---

## Система страха

Страх — значение от 0 до 100, хранится у каждого игрока через Capability API.

### Пороги:

| Уровень | Значение | Поведение мобов |
|---------|----------|-----------------|
| Низкий  | 0–39     | Обычное поведение |
| Агрессия | 40–69   | Хостайлы атакуют, нейтралы преследуют |
| Гипер   | 70–100   | Мирные мобы убегают, все агрессивны |

### Зоны влияния:

```
0–12 блоков   → полное влияние страха
12–16 блоков  → затухание
16–32 блока   → постепенный сброс
32+ блоков    → полный сброс преследования
```

### Поведение по типам мобов:

**Hostile (зомби, скелеты)**
- Страх ≥ 40 → агрессивно атакуют игрока

**Neutral (волки, эндермены)**
- Страх ≥ 40 → ведут себя как хостайлы
- Страх 20–39 → держат дистанцию, наблюдают

**Peaceful (коровы, овцы)**
- Страх ≥ 70 → убегают на 12 блоков
- Страх 40–69 → убегают при сближении до 6 блоков

---

## Интеграция с Simple Voice Chat

Когда игрок говорит в войс-чат — его уровень страха повышается.

```
Игрок говорит → ClientVoiceIntegration слушает микрофон
             → C2S пакет отправляется на сервер
             → VoiceFearHandler повышает страх
             → FearMobAIHandler применяет эффекты к мобам
```

**Требуется:** [Simple Voice Chat](https://www.curseforge.com/minecraft/mc-mods/simple-voice-chat)

---

## Структура

```
src/main/java/com/example/soundattract/
├── fear/
│   ├── IFear.java                  — интерфейс Capability
│   ├── FearCapability.java         — реализация (0–100)
│   ├── FearTickHandler.java        — тик-хендлер
│   ├── FearMobAIHandler.java       — AI мобов по страху
│   ├── FearUtils.java              — утилиты
│   ├── FearSystemState.java        — глобальный on/off
│   ├── ClientVoiceIntegration.java — интеграция войс-чата
│   └── VoiceFearHandler.java       — серверная обработка
├── ai/
│   ├── AttractionGoal.java
│   ├── LeaderAttractionGoal.java
│   ├── MobGroupManager.java
│   └── TeleportToSoundGoal.java
└── raid/
    └── RaidReinforcementManager.java
```

---

##  Сборка

```bash
./gradlew build
```

Готовый `.jar` в `build/libs/`

---

*Форк AttractToSound · Fear System добавлен NECLOCER DEV · 2025*
*Telegram: @neclocer*
