# Встановлення DayZ Dedicated Server: покроково

Виконуйте кроки строго по черзі. Для кожного кроку є варіант для **Windows** і для **Linux** (Ubuntu/Debian).

---

## Крок 0. Підготовка
- Сервер або ПК: від 4 ядер, 8–16 ГБ RAM, SSD, близько 20 ГБ вільного місця.
- Steam-акаунт, **на якому куплена DayZ**. Без нього файли сервера не завантажаться.
- Білий (публічний) IP або VPS. На домашньому ПК потрібен доступ до роутера, щоб прокинути порти.

## Крок 1. Встановити залежності
**Windows:** встановіть [Microsoft Visual C++ Redistributable x64](https://aka.ms/vs/17/release/vc_redist.x64.exe) і DirectX End-User Runtime.

**Linux:**
```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install -y lib32gcc-s1 steamcmd
sudo useradd -m -s /bin/bash dayz      # окремий користувач для сервера
sudo -iu dayz
```

## Крок 2. Встановити SteamCMD
**Windows:** створіть папку `C:\steamcmd`, завантажте туди
[steamcmd.zip](https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip), розпакуйте і один раз запустіть `steamcmd.exe`, щоб він оновився.

**Linux:** уже встановлено на кроці 1, команда `steamcmd`.

## Крок 3. Завантажити файли сервера (App ID 223350)
**Windows:**
```bat
C:\steamcmd\steamcmd.exe +force_install_dir C:\DayZServer +login ВАШ_ЛОГІН +app_update 223350 validate +quit
```
**Linux:**
```bash
steamcmd +force_install_dir ~/dayzserver +login ВАШ_ЛОГІН +app_update 223350 validate +quit
```
SteamCMD попросить пароль і код Steam Guard. Завантаження займе кілька хвилин.

## Крок 4. Налаштувати `serverDZ.cfg`
Відкрийте `serverDZ.cfg` у папці сервера і змініть щонайменше ці параметри:
```cpp
hostname = "Назва вашого сервера";
password = "";                 // пароль для входу гравців (порожній = вільний вхід)
passwordAdmin = "СильнийПароль";
maxPlayers = 60;
steamQueryPort = 27016;
class Missions {
    class DayZ { template = "dayzOffline.chernarusplus"; }; // Livonia: dayzOffline.enoch, Sakhal: dayzOffline.sakhal
};
```

## Крок 5. Налаштувати BattlEye RCon (для адміністрування)
Створіть файл `battleye/BEServer_x64.cfg`:
```
RConPassword ІншийСильнийПароль
RConPort 2310
RestrictRCon 0
```
Після запуску BattlEye перейменує цей файл на `BEServer_x64_active_*.cfg`. Так і має бути.

## Крок 6. Відкрити порти
Потрібні порти: UDP 2302–2305 (гра), UDP 27016 (видимість у списку серверів), UDP 2310 (RCon).

**Windows** (PowerShell від імені адміністратора):
```powershell
New-NetFirewallRule -DisplayName "DayZ Server" -Direction Inbound -Protocol UDP -LocalPort 2302-2305,2310,27016 -Action Allow
```
**Linux:**
```bash
sudo ufw allow 2302:2305/udp
sudo ufw allow 27016/udp
sudo ufw allow 2310/udp
```
Якщо сервер стоїть удома, прокиньте ці самі порти на роутері до локального IP цього ПК.

## Крок 7. Створити скрипт запуску
**Windows**, файл `C:\DayZServer\start.bat`:
```bat
@echo off
cd /d C:\DayZServer
start "DayZ" DayZServer_x64.exe -config=serverDZ.cfg -port=2302 -profiles=profiles -BEpath=battleye -dologs -adminlog -netlog -freezecheck
```
**Linux**, файл `~/dayzserver/start.sh`:
```bash
#!/bin/bash
cd ~/dayzserver
./DayZServer -config=serverDZ.cfg -port=2302 -profiles=profiles \
  -BEpath=battleye -dologs -adminlog -netlog -freezecheck
```
Потім зробіть його виконуваним: `chmod +x ~/dayzserver/start.sh`.

## Крок 8. Перший запуск і перевірка
1. Запустіть `start.bat` або `./start.sh`.
2. Зачекайте 1–3 хвилини, поки завантажиться світ.
3. Перевірте логи в папці `profiles` (файли `*.RPT`, `*.ADM`). Якщо сервер падає, причину видно в кінці RPT-файлу.

## Крок 9. Підключитися з гри
DayZ Launcher → **Servers** → вкладка **LAN** (якщо ви в тій самій мережі) або **Community** і пошук за назвою.
Інший варіант: Steam → Вигляд → Ігрові сервери → Обране → Додати `ВАШ_IP:27016`.

## Крок 10. Моди (необов'язково)
1. Завантажте мод: `steamcmd +login ВАШ_ЛОГІН +workshop_download_item 221100 <ID_моду> +quit`.
2. Скопіюйте папку мода з `steamapps/workshop/content/221100/<ID>` у корінь сервера під назвою на кшталт `@CF`. На Linux назви файлів і папок мають бути в нижньому регістрі.
3. Скопіюйте `*.bikey` з папки `keys` мода в папку `keys` сервера.
4. Додайте до скрипту запуску `-mod=@CF;@ІншийМод`. Порядок модів важливий: залежності вказуються першими.

## Крок 11. Обслуговування
- **Оновлення:** зупиніть сервер і повторіть команду з кроку 3. Робіть це після кожного патчу DayZ.
- **Рестарти:** налаштуйте автоматичні рестарти кожні 4–6 годин (`messages.xml` у місії або планувальник/systemd).
- **Бекапи:** регулярно копіюйте `mpmissions/<місія>/storage_1` — там прогрес гравців і стан світу.
