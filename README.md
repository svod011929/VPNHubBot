# MULTI VPN: Телеграм-бот для управления VPN

## Быстрый старт по установке и настройке

---

### ⚠ Важно:  
Этот репозиторий служит только руководством по установке и настройке. Для приобретения исходного кода, пишите в Telegram: [@KodoDrive](https://t.me/KodoDrive)  
Демо-бот: [@fx_vpnshop_bot](https://t.me/fx_vpnshop_bot)

---

## 1. Аренда VPS для бота и VPN

**Рекомендации по выбору сервера:**
- **Диск:** не менее 20 ГБ, если планируете запускать бота и VPN на одном сервере.
- **Локация:** выбирайте как можно ближе к пользователям (Европа: Амстердам, Турция и др.).
- **Скорость:** от 200 Мбит/с — на каждого 100–120 пользователей.
- **ОС:** Ubuntu 22.
- **Ресурсы:** для чистого VPN хватит минимального процессора и ОЗУ; для бота — 1 ГБ ОЗУ.

**Варианты VPS:**  
[Profitserver — 330₽](https://ps.profitserver.pro/)  
[JustHost — 500₽](https://justhost.ru/)

---

## 2. Подключение к серверу

После оплаты VPS вы получите IP и пароль.

### Клиенты SSH:
- **Windows:** [MobaXterm](https://mobaxterm.mobatek.net/download.html)
- **Linux/Mac:** [WindTerm](https://github.com/kingToolbox/WindTerm/releases/tag/2.6.0) — скачайте подходящую версию внизу страницы.

#### WindTerm — инструкция:

  1. Откройте "Сессия" → "Новая сессия"
  2. Выберите SSH
  3. В поле "Хозяин (H)" — IP сервера
  4. Нажмите "Соединить"
  5. На вкладке "Account" — пользователь: root, пароль: из письма

#### MobaXterm — инструкция:

  1. "Session" → SSH → "Remote host": IP сервера
  2. ОК → выберите сервер — начнётся подключение
  3. Логин: root, пароль: из письма (ввод скрыт для защиты)

---

## 3. Установка и запуск Telegram-бота

Выполните команды по шагам:

1. Обновите стандартные пакеты:
   ```
   sudo apt update && sudo apt upgrade -y && sudo apt install curl && apt-get install gettext
   ```

2. Установите Docker:
   ```
   curl -fSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh && sudo apt install docker-compose-plugin
   ```

3. Передайте на сервер папку `VPNHubBot` из архива
4. Перейдите в папку:
   ```
   cd VPNHubBot
   ```

5. Откройте и заполните файл `.env` — основные поля:
   - NAME: Имя бота
   - LANGUAGES: ru или en
   - ADMIN_TG_ID: ваш Telegram ID ([https://t.me/userinfobot](https://t.me/userinfobot))
   - MONTH_COST: цены подписки в рублях (через запятую)
   - DEPOSIT: суммы пополнения баланса
   - TRIAL_PERIOD: пробный период (в секундах)
   - TG_TOKEN: бот-токен ([https://t.me/BotFather](https://t.me/BotFather))
   - YOOMONEY, LAVA, YOOKASSA, CRYPTOMUS, CryptoBot API, и др. — токены и реквизиты платёжных сервисов, берите/создавайте по инструкциям в комментариях файла

   **Внимание:** не используйте спецсимволы в логинах/паролях!

6. Настройте оформление и описание в BotFather:  
   Перейдите в меню /mybots → выберите своего бота → Edit Bot

7. Запуск бота из папки:
   ```
   sudo docker compose up -d
   ```
   **Остановить:**
   ```
   sudo docker compose down
   ```

8. Админ-панель доступна через меню бота. Если кнопка не видна — прокрутите клавиатуру.

---

## 4. Работа с ошибками

Для просмотра логов:
```
docker compose logs -f
```
Запись логов в файл для последующего анализа:
```
docker logs vpnhubbot-vpn_hub_bot-1 > bot_all.log 2>&1 &
```

---

## 5. Редактирование текстов бота

1. Перейдите в директорию бота:
   ```
   cd VPNHubBot
   ```
2. Остановите бота:
   ```
   sudo docker compose down
   ```
3. Откройте папку `locale` и нужный язык (ru, en, ...), отредактируйте файл `bot.po`
4. Скомпилируйте переводы:
   ```
   sh compile_translations.sh
   ```
5. Обновите образы:
   ```
   sudo docker compose build
   ```
6. Запустите бота:
   ```
   sudo docker compose up -d
   ```

**Этот порядок действий обязателен при каждом изменении текстов!**

---

## 6. Установка VPN VLESS (Reality)

**Настройка под VLESS + Reality:**

1. Обновите систему (только для новых серверов):
   ```
   apt update && apt install curl -y
   ```
2. Установите панель XU-I:
   ```
   bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
   ```
3. Следуйте подсказкам скрипта:
   ```
   - Do you want to continue? → y
   - Panel port: укажите свой (любые 4 цифры)
   - Web base path: Enter
   ```
4. Получите логин/пароль/путь, которые покажет скрипт
5. В браузере откройте:
   ```
   http://<vps_ip>:<port>/<path>
   ```
6. "Подключения" → "Добавить подключение":
   - Выберите Reality
   - GET NEW KEY
   - Создать
7. В настройках можно изменить логин, пароль, webPath

---

## 7. Установка ShadowSocks

Повторите первые 6 шагов из установки VLESS — панели XU-I.

1. "Добавить подключение"
2. Протокол: shadowsocks — создать подключение  
Вы можете переименовать пользователя на admin

---

## 8. Установка Outline

1. (Только для новых серверов)  
   ```
   apt update && apt install curl -y
   ```
2. Установите Outline:
   ```
   sudo wget -qO- https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh | bash
   ```
   **Примечание:** При необходимости Docker ставьте отдельно:
   ```
   sudo curl https://get.docker.com | sh
   ```
   После завершения скрипта появятся данные:
   ```
   { "apiUrl": ..., "certSha256": ... }
   ```
   Сохраните их для настройки бота.

---

## 9. Добавление VPN-сервера в бота

1. В боте: "Сервера" → "Добавить сервер"
2. Задайте уникальное имя (можно с эмодзи)
3. Введите IP и порт (при необходимости — webPath)
4. Пароль от сервера — любое значение (бот не использует)
5. Выберите тип VPN
6. Для Outline введите apiUrl & certSha256 (копируйте целиком)
7. Для VLESS/ShadowSocks — продолжайте по инструкции
8. Тип подключения: HTTP
9. Панель: Sanae
10. ID подключения — в панели, рядом с "+" (Connect ID)
11. Логин/пароль из панели
12. Если на сервере были клиенты — сперва удалите старые подключения

---

## 10. Веб-панель управления базой данных (pgAdmin)

> ⚠ Все действия в панели — только на свой страх и риск!

1. Откройте в браузере:
   ```
   http://<vps_ip>:5050/
   ```
2. Email и пароль — как указано в .env (PGADMIN_DEFAULT_EMAIL / PGADMIN_DEFAULT_PASSWORD)
3. "Добавить новый сервер"
4. Имя: admin
5. "Соединение":
   - Имя/адрес сервера: db_postgres
   - Порт: 5432
   - Служебная база: значение POSTGRES_DB
   - Имя пользователя: POSTGRES_USER
   - Пароль: POSTGRES_PASSWORD

---

## ✅ Всё готово!  
Бот установлен, VPN настроен — начинайте пользоваться!
