<p align="center">
  <img src="https://img.shields.io/badge/RUnion%20Ladder%20Bot-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="RUnion Ladder Bot">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/discord.py-2.3%2B-5865F2?logo=discord&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/WoW-3.3.5%20WotLK-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge">
</p>

<h1 align="center">⛏️ RUnion Ladder Bot</h1>

<p align="center">
  <b>Discord-бот для рейтингов гильдии с UwU Logs.</b><br>
  <i>Builds a beautiful guild ladder in Discord with class/spec filters, pagination, and position trends.</i>
</p>

<p align="center">
  <a href="#english">English</a> ·
  <a href="#russian">Русский</a>
</p>

---

<a name="english"></a>
## English

<p align="center">
  <a href="#features">Features</a> ·
  <a href="#commands">Commands</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#configuration">Configuration</a> ·
  <a href="#project-structure">Structure</a> ·
  <a href="#faq">FAQ</a> ·
  <a href="#license">License</a>
</p>

### About

**RUnion Ladder Bot** pulls guild member ratings from **UwU Logs** and builds an interactive ladder inside Discord — with class/spec filters, pagination, position trends, and custom badges.

### Features

- 📊 **Guild ladder** sorted by UwU Logs rating (Best metric), best to worst.
- 🎯 **Interactive filters** inside the message: class filter, spec selector, pagination (25 per page) — no re-typing commands.
- 🏆 **Best spec selection:** all DPS specs are checked per player, the highest result is used.
- 🩸 **Hybrids included:** Blood DK, Feral Druid etc. make the ladder; pure healers/tanks are filtered out.
- 📈 **Position trends:** bot remembers previous rankings and shows who climbed/dropped/is new.
- 👑 **Named badges:** special nicks get a crown.
- 🗂️ **Roster management from Discord:** `/roster add | remove | list`.
- 🌐 **Multi-server:** commands can be registered across multiple Discord servers.
- 💾 **Disk cache:** instant ladder output, auto-rebuild on TTL and roster changes.
- ⚙️ **Two engines:** `api` (fast, recommended) and `browser` (headless).

### Screenshots

<p align="center">
  <img width="500" alt="/ladder" src="docs/ladder.png">
  <br>
  <i>/ladder — guild top with filters and pagination</i>
</p>

<p align="center">
  <img width="500" alt="/runion" src="docs/runion.png">
  <br>
  <i>/runion — welcome and help</i>
</p>

<a name="commands"></a>
### Commands

- `/ladder` — show the guild ladder; filters and pagination via buttons (everyone).
- `/runion` — welcome and help with commands and badges (everyone).
- `/roster add nick:<name>` — add a player to the roster (everyone).
- `/roster remove nick:<name>` — remove a player from the roster (mods and admins with Manage Server).
- `/roster list` — show the current roster size (everyone).

#### Ladder badges

- 🥇 🥈 🥉 — guild top-3.
- 👑 — named player (custom badge).
- 🔺N — climbed N positions.
- 🔻N — dropped N positions.
- 🆕 — new in the ladder.
- ➖ — no change.

Class icons: 💀 DK · 🌿 Druid · 🏹 Hunter · ❄️ Mage · 🔨 Paladin · 🙏 Priest · 🗡️ Rogue · ⚡ Shaman · 🔥 Warlock · ⚔️ Warrior

<a name="quick-start"></a>
### Quick Start

```bash
git clone https://github.com/Keoo888/uwu-logs.xyz-discord-ladder-bot.git
cd uwu-logs.xyz-discord-ladder-bot
pip install -r requirements.txt
cp .env.example .env
#  -> edit .env: set DISCORD_TOKEN, DISCORD_GUILD_ID
python -u -B bot.py
```

Expected output:
```
RUnion Ladder Bot | 2026-07-05.38-local ... | UWU_MODE=api
Commands synced for server <GUILD_ID>
Logged in as RUnion bot#1120 (id=...)
```

<a name="configuration"></a>
### Configuration (`.env`)

- `DISCORD_TOKEN` — **required**; bot token from the Developer Portal.
- `DISCORD_GUILD_ID` — server ID(s) for instant commands; separate multiple IDs with commas (`111,222`); empty means global (up to one hour).
- `WARMANE_GUILD` — guild name; default `RUnion`.
- `WARMANE_REALM` — realm name; default `Icecrown`.
- `UWU_SERVER` — realm for UwU Logs; default `Icecrown`.
- `UWU_MODE` — engine; default `browser`, `api` is recommended.
- `UWU_SPECS` — talent specs to check; default `1,2,3`; the best result is taken.
- `LADDER_LIMIT` — default ladder rows; default `50`.
- `CACHE_TTL` — cache lifetime in seconds; default `3600` (one hour).
- `UWU_REQUEST_DELAY` — delay between UwU requests in seconds; default `1.0`.
- `UWU_CONCURRENCY` — concurrent requests in `browser` mode; default `2`.
- `UWU_API_CONCURRENCY` — concurrent requests in `api` mode; default `8`.
- `UWU_PRUNE_ROSTER` — `1` automatically removes players that are not found; default `0`.

> Full list with comments in [`.env.example`](.env.example).

#### Roster

[`roster.txt`](roster.txt) — one player per line:
```
Name | Class      # class is optional, used for icon
Anatolevich | Warrior
Esno | Hunter
```

Lines starting with `#` and empty lines are ignored. Edit via Discord with `/roster add | remove`.

<a name="project-structure"></a>
### Project Structure

- `bot.py` — Discord bot: commands, buttons, embeds.
- `ladder.py` — ladder builder, cache and version.
- `uwu.py` — UwU Logs requests (API and browser) and best-spec lookup.
- `roster.py` — roster read and write.
- `store.py` — disk cache, position history and best specs.
- `config.py` — configuration from `.env`.
- `discover.py` — UwU API endpoint discovery helper.
- `test_api.py` — quick API-mode test.
- `roster.txt` — player list.

<a name="faq"></a>
### FAQ

**`/ladder` shows old parameters.** Restart Discord client (Ctrl+R) to refresh command list.

**First ladder build is slow.** Full UwU scan (~3–4 min). Subsequent runs use cache; rebuild on TTL (1hr) and roster changes.

**`Expecting value: line 1 column 1` / empty responses.** UwU throttles under high load — keep `UWU_API_CONCURRENCY` at 8 or below.

**Player not in ladder.** Check nickname in `roster.txt` (case-sensitive, must match Warmane/UwU) and ensure they have DPS logs on UwU.

<a name="license"></a>
### License

MIT — do whatever you want, but credit the author. See [LICENSE](LICENSE).

Created by: **Egormashina**

---

<a name="russian"></a>
## Русский

<p align="center">
  <a href="#особенности">Особенности</a> ·
  <a href="#команды">Команды</a> ·
  <a href="#быстрый-старт">Быстрый старт</a> ·
  <a href="#настройка">Настройка</a> ·
  <a href="#устройство-проекта">Устройство</a> ·
  <a href="#faq-1">FAQ</a> ·
  <a href="#лицензия">Лицензия</a>
</p>

### Об аддоне

**RUnion Ladder Bot** тянет рейтинги игроков гильдии с **UwU Logs** и строит красивый ладдер прямо в Discord — с фильтрами по классу/спеку, листалкой и трендами позиций.

<a name="особенности"></a>
### Особенности

- 📊 **Ладдер гильдии** по рейтингу UwU Logs (метрика **Best**), от лучших к худшим.
- 🎯 **Интерактивные фильтры** прямо в сообщении: выбор класса, спека, листание страниц (по 25 строк) — без повторного ввода команды.
- 🏆 **Максимум по спекам:** для каждого игрока проверяются все ДПС-ветки и берётся лучший результат.
- 🩸 **Хибриды учитываются:** Blood DK, Feral Druid и подобные попадают в ладдер; чистые хилы/танки отсеиваются.
- 📈 **Тренды позиций:** бот запоминает прошлые места и показывает, кто поднялся/опустился/новичок.
- 👑 **Именные значки:** особые ники подсвечиваются короной.
- 🗂️ **Управление ростером из Discord:** `/roster add | remove | list`.
- 🌐 **Несколько серверов:** команды регистрируются сразу на нескольких Discord-серверах.
- 💾 **Дисковый кеш:** мгновенный вывод ладдера, авто-пересбор по TTL и после правок ростера.
- ⚙️ **Два движка сбора:** `api` (быстрый, рекомендуется) и `browser` (через headless-браузер).

### Скриншоты

<p align="center">
  <img width="500" alt="/ladder" src="docs/ladder.png">
  <br>
  <i>/ladder — топ гильдии с фильтрами и листалкой</i>
</p>

<p align="center">
  <img width="500" alt="/runion" src="docs/runion.png">
  <br>
  <i>/runion — приветствие и справка</i>
</p>

<a name="команды"></a>
### Команды

- `/ladder` — показать ладдер гильдии; фильтры и листалка кнопками (все).
- `/runion` — приветствие и справка по командам и значкам (все).
- `/roster add nick:<ник>` — добавить игрока в ростер (все).
- `/roster remove nick:<ник>` — убрать игрока из ростера (модеры и админы с Manage Server).
- `/roster list` — сколько игроков в ростере (все).

#### Значки в ладдере

- 🥇 🥈 🥉 — топ-3 гильдии.
- 👑 — «наш человек» (именной значок).
- 🔺N — поднялся на N позиций.
- 🔻N — опустился на N позиций.
- 🆕 — новичок в ладдере.
- ➖ — без изменений.

Иконки классов: 💀 DK · 🌿 Druid · 🏹 Hunter · ❄️ Mage · 🔨 Paladin · 🙏 Priest · 🗡️ Rogue · ⚡ Shaman · 🔥 Warlock · ⚔️ Warrior

<a name="быстрый-старт"></a>
### Быстрый старт

```bash
git clone https://github.com/Keoo888/uwu-logs.xyz-discord-ladder-bot.git
cd uwu-logs.xyz-discord-ladder-bot
pip install -r requirements.txt
cp .env.example .env
#  -> открой .env и впиши DISCORD_TOKEN и DISCORD_GUILD_ID
python -u -B bot.py
```

В консоли должно появиться:
```
RUnion Ladder Bot | 2026-07-05.38-local ... | UWU_MODE=api
Команды синхронизированы для сервера <GUILD_ID>
Вошёл как RUnion bot#1120 (id=...)
```

<a name="настройка"></a>
### Настройка (`.env`)

- `DISCORD_TOKEN` — **обязательно**; токен бота из Developer Portal.
- `DISCORD_GUILD_ID` — ID сервера или серверов для мгновенной регистрации; несколько ID — через запятую (`111,222`); пусто — глобально (до часа).
- `WARMANE_GUILD` — название гильдии; по умолчанию `RUnion`.
- `WARMANE_REALM` — реалм; по умолчанию `Icecrown`.
- `UWU_SERVER` — реалм для UwU Logs; по умолчанию `Icecrown`.
- `UWU_MODE` — движок; по умолчанию `browser`, рекомендуется `api`.
- `UWU_SPECS` — какие ветки талантов проверять; по умолчанию `1,2,3`; берётся лучший результат.
- `LADDER_LIMIT` — строк в ладдере по умолчанию; `50`.
- `CACHE_TTL` — время жизни кеша в секундах; по умолчанию `3600` (один час).
- `UWU_REQUEST_DELAY` — пауза между запросами к UwU в секундах; по умолчанию `1.0`.
- `UWU_CONCURRENCY` — одновременные запросы в `browser`-режиме; по умолчанию `2`.
- `UWU_API_CONCURRENCY` — одновременные запросы в `api`-режиме; по умолчанию `8`.
- `UWU_PRUNE_ROSTER` — `1` автоматически удаляет ненайденных игроков; по умолчанию `0`.

> Полный список с комментариями — в [`.env.example`](.env.example).

#### Ростер

Файл [`roster.txt`](roster.txt) — по одному игроку на строку:
```
Имя | Класс      # класс нужен только для иконки, можно не указывать
Anatolevich | Warrior
Esno | Hunter
```

Строки с `#` и пустые игнорируются. Ростер можно править из Discord через `/roster add | remove`.

<a name="устройство-проекта"></a>
### Устройство проекта

- `bot.py` — Discord-бот: команды, кнопки, эмбеды.
- `ladder.py` — сборка ладдера, кеш и версия.
- `uwu.py` — запросы к UwU Logs (API и browser) и поиск лучшего спека.
- `roster.py` — чтение и запись ростера.
- `store.py` — дисковый кеш, история позиций и лучшие спеки.
- `config.py` — настройки из `.env`.
- `discover.py` — поиск API-эндпоинта UwU.
- `test_api.py` — быстрая проверка API-режима.
- `roster.txt` — список игроков гильдии.

<a name="faq-1"></a>
### Частые вопросы

**`/ladder` показывает старые параметры.** Перезапусти клиент Discord (Ctrl+R) — список команд обновится.

**Ladder долго собирается в первый раз.** Это нормально: полный скан UwU (~3–4 мин). Дальше из кеша мгновенно; пересбор — по TTL (1 ч) и после правок ростера.

**`Expecting value: line 1 column 1` / пустые ответы.** UwU троттлит при нагрузке — не поднимай `UWU_API_CONCURRENCY` выше 8.

**Игрока нет в ладдере.** Проверь ник в `roster.txt` (регистр как на Warmane/UwU) и что у него есть ДПС-логи на UwU.

<a name="лицензия"></a>
### Лицензия

MIT — делай что хочешь, но упоминай автора. Подробности — в [LICENSE](LICENSE).

Создатель: **Egormashina**
