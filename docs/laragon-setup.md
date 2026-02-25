# Настройка WordPress на Laragon

## Содержание

1. [Требования](#требования)
2. [Установка Laragon](#установка-laragon)
3. [Настройка проекта](#настройка-проекта)
4. [Создание базы данных](#создание-базы-данных)
5. [Установка WordPress](#установка-wordpress)
6. [Настройка wp-config.php](#настройка-wp-configphp)
7. [Дополнительные возможности Laragon](#дополнительные-возможности-laragon)
8. [Устранение проблем](#устранение-проблем)

---

## Требования

- **ОС:** Windows 7/8/10/11 (64-bit)
- **Laragon:** версия 5+ или 6+ (Full или Wamp)
- **PHP:** 8.1 или выше (встроен в Laragon)
- **MySQL/MariaDB:** встроен в Laragon
- **Свободное место:** минимум 500 МБ

---

## Установка Laragon

### Шаг 1: Скачивание

1. Перейдите на официальный сайт: [https://laragon.org](https://laragon.org)
2. Скачайте **Laragon Full** (рекомендуется) или **Laragon Wamp**
3. Запустите установщик

### Шаг 2: Установка

1. Выберите язык установки
2. Примите лицензионное соглашение
3. Выберите папку установки (по умолчанию: `C:\laragon`)
4. Выберите компоненты:
   - ✅ Apache/Nginx
   - ✅ MySQL/MariaDB
   - ✅ PHP 8.x
   - ✅ phpMyAdmin
   - ✅ Git
   - ✅ Composer
   - ✅ Node.js (опционально)
5. Завершите установку

### Шаг 3: Первый запуск

1. Запустите Laragon из меню Пуск или с рабочего стола
2. Нажмите **Start All** (зелёная кнопка)
3. Дождитесь запуска всех служб
4. Индикаторы станут зелёными

---

## Настройка проекта

### Способ 1: Автоматическое создание (рекомендуется)

Laragon поддерживает функцию **Auto Virtual Hosts**:

1. Откройте папку веб-сервера: `C:\laragon\www`
2. Создайте папку с именем проекта:
   ```
   C:\laragon\www\wordpress-6.9.1.local
   ```
3. Скопируйте файлы WordPress в эту папку
4. Laragon автоматически создаст виртуальный хост
5. Перезапустите Apache/Nginx (кнопка **Stop All** → **Start All**)

Теперь сайт доступен по адресу: `http://wordpress-6.9.1.local`

### Способ 2: Ручное создание виртуального хоста

1. Откройте меню Laragon → **Apache** → **sites-enabled** → **Create Virtual Host**
2. Введите имя домена: `wordpress-6.9.1.local`
3. Укажите путь к папке проекта
4. Нажмите **OK**
5. Перезапустите веб-сервер

### Способ 3: Быстрое создание через хоткей

1. Нажмите `Ctrl + Alt + N` (создать новый проект)
2. Введите имя проекта: `wordpress-6.9.1`
3. Laragon создаст папку и виртуальный хост автоматически
4. Скопируйте файлы WordPress в созданную папку

---

## Создание базы данных

### Способ 1: Через phpMyAdmin

1. Откройте браузер, перейдите: `http://localhost/phpmyadmin`
2. Авторизуйтесь (по умолчанию: пользователь `root`, пароль пустой)
3. Нажмите вкладку **Базы данных** (Databases)
4. Введите имя базы: `wordpress`
5. Выберите кодировку: `utf8mb4_unicode_ci`
6. Нажмите **Создать** (Create)

### Способ 2: Через консоль MySQL

1. Откройте терминал Laragon (кнопка **Terminal** или `Ctrl + Alt + T`)
2. Подключитесь к MySQL:
   ```bash
   mysql -u root -p
   ```
   (пароль по умолчанию пустой — просто нажмите Enter)

3. Выполните команды:
   ```sql
   CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   SHOW DATABASES;
   EXIT;
   ```

### Способ 3: Через Quick App (автоматически)

При установке WordPress через встроенную функцию Laragon база данных создаётся автоматически.

### Учётные данные по умолчанию

| Параметр | Значение |
|----------|----------|
| Хост | `localhost` |
| Порт | `3306` |
| Пользователь | `root` |
| Пароль | (пустой) |

---

## Установка WordPress

### Способ 1: Ручная установка

1. Откройте браузер
2. Перейдите: `http://wordpress-6.9.1.local`
3. Выберите язык (русский)
4. Нажмите **Продолжить**
5. Заполните данные:
   - Название сайта
   - Имя пользователя (admin)
   - Пароль
   - Email
6. Нажмите **Установить WordPress**
7. Войдите в админ-панель

### Способ 2: Автоматическая установка через Laragon

Laragon имеет встроенную функцию быстрой установки WordPress:

1. Кликните правой кнопкой по иконке Laragon в трее
2. Выберите **Quick App** → **WordPress**
3. Введите имя проекта
4. Laragon автоматически:
   - Скачает последнюю версию WordPress
   - Создаст базу данных
   - Настроит `wp-config.php`
   - Запустит установку

---

## Настройка wp-config.php

### Базовая конфигурация для Laragon

```php
<?php
// ** Database settings ** //
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'root' );
define( 'DB_PASSWORD', '' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8mb4' );
define( 'DB_COLLATE', '' );

// ** Ключи безопасности ** //
define( 'AUTH_KEY',         'уникальная-фраза-1' );
define( 'SECURE_AUTH_KEY',  'уникальная-фраза-2' );
define( 'LOGGED_IN_KEY',    'уникальная-фраза-3' );
define( 'NONCE_KEY',        'уникальная-фраза-4' );
define( 'AUTH_SALT',        'уникальная-фраза-5' );
define( 'SECURE_AUTH_SALT', 'уникальная-фраза-6' );
define( 'LOGGED_IN_SALT',   'уникальная-фраза-7' );
define( 'NONCE_SALT',       'уникальная-фраза-8' );

// ** Префикс таблиц ** //
$table_prefix = 'wp_';

// ** Отладка ** //
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );

// ** Пути ** //
if ( ! defined( 'ABSPATH' ) ) {
    define( 'ABSPATH', __DIR__ . '/' );
}

require_once ABSPATH . 'wp-settings.php';
```

### Генерация ключей безопасности

Посетите: [https://api.wordpress.org/secret-key/1.1/salt/](https://api.wordpress.org/secret-key/1.1/salt/)

Скопируйте сгенерированные ключи и замените ими строки в `wp-config.php`

---

## Дополнительные возможности Laragon

### Смена версии PHP

1. Скачайте нужную версию PHP с [windows.php.net](https://windows.php.net)
2. Распакуйте в папку: `C:\laragon\bin\php\php-8.x.x`
3. Меню Laragon → **PHP** → Выберите версию
4. Перезапустите сервер

### Включение расширений PHP

1. Откройте: `C:\laragon\bin\php\php-8.x.x\php.ini`
2. Найдите нужное расширение (например, `;extension=curl`)
3. Уберите точку с запятой: `extension=curl`
4. Перезапустите Laragon

### Настройка почтового сервера

Laragon включает встроенный MailHog для тестирования отправки писем:

1. Меню Laragon → **MailHog** → **Start**
2. Откройте: `http://localhost:8025`
3. Все письма будут отображаться в веб-интерфейсе

### Использование разных версий MySQL

1. Скачайте нужную версию MySQL/MariaDB
2. Распакуйте в: `C:\laragon\bin\mysql\`
3. Меню Laragon → **MySQL** → Выберите версию
4. Перезапустите

### Быстрый доступ к папкам

- `Ctrl + Alt + E` — открыть папку `www`
- `Ctrl + Alt + L` — открыть логи
- `Ctrl + Alt + N` — создать новый проект

### Терминал

Laragon включает предустановленный терминал с Git, Composer, NPM:

- Кнопка **Terminal** или `Ctrl + Alt + T`
- Все команды доступны сразу

### Авто-HTTPS

Laragon автоматически настраивает HTTPS для всех проектов:

- `http://wordpress-6.9.1.local` — HTTP
- `https://wordpress-6.9.1.local` — HTTPS (самоподписанный сертификат)

---

## Устранение проблем

### Ошибка подключения к базе данных

**Проблема:** `Error establishing a database connection`

**Решения:**
1. Убедитесь, что MySQL запущен (зелёный индикатор)
2. Проверьте, что БД создана в phpMyAdmin
3. Проверьте данные в `wp-config.php`
4. Попробуйте `127.0.0.1` вместо `localhost`

### Порт 80 занят

**Проблема:** Apache не запускается, порт 80 занят

**Решения:**
1. Закройте Skype, IIS или другие службы, использующие порт 80
2. Или измените порт в настройках Laragon:
   - Меню → **Настройки** → **Порт Apache**: 8080
   - Тогда сайт будет доступен по: `http://wordpress-6.9.1.local:8080`

### Порт 3306 занят

**Проблема:** MySQL не запускается

**Решения:**
1. Закройте другие экземпляры MySQL
2. Измените порт MySQL в настройках Laragon

### Белый экран смерти

**Проблема:** Пустой белый экран

**Решения:**
1. Включите отладку в `wp-config.php`:
   ```php
   define( 'WP_DEBUG', true );
   define( 'WP_DEBUG_DISPLAY', true );
   ```
2. Проверьте `wp-content/debug.log`
3. Увеличьте лимит памяти:
   ```php
   define( 'WP_MEMORY_LIMIT', '256M' );
   ```

### Ошибка 404 на страницах

**Проблема:** Работает только главная страница

**Решения:**
1. В админке: **Настройки** → **Постоянные ссылки** → Сохранить
2. Проверьте `.htaccess` в корне WordPress
3. Убедитесь, что mod_rewrite включен в Apache

### Проблемы с загрузкой файлов

**Решения:**
1. Откройте `php.ini` (меню Laragon → **PHP** → **php.ini**)
2. Найдите и измените:
   ```ini
   upload_max_filesize = 64M
   post_max_size = 64M
   max_execution_time = 300
   memory_limit = 256M
   ```
3. Перезапустите Laragon

### Медленная работа

**Решения:**
1. Включите OPcache в `php.ini`:
   ```ini
   opcache.enable=1
   opcache.memory_consumption=256
   ```
2. Установите плагин кэширования
3. Отключите неиспользуемые плагины

---

## Полезные команды

### Перезапуск Laragon

```
# Через интерфейс
Stop All → Start All

# Через консоль (от администратора)
net stop wampapache64
net stop wampmysqld64
net start wampapache64
net start wampmysqld64
```

### Сброс кэша OPcache

1. Меню Laragon → **PHP** → **Restart PHP**

### Просмотр логов

- **Apache:** `C:\laragon\logs\apache_error.log`
- **MySQL:** `C:\laragon\logs\mysql_error.log`
- **WordPress:** `C:\laragon\www\wordpress-6.9.1.local\wp-content\debug.log`

### Консоль MySQL

```bash
# Подключение
mysql -u root -p

# Создание БД
CREATE DATABASE wordpress CHARACTER SET utf8mb4;

# Просмотр всех БД
SHOW DATABASES;

# Выход
EXIT;
```

### Composer в Laragon

```bash
# Установка зависимостей
composer install

# Обновление
composer update

# Установка плагина через Composer
composer require vendor/package
```

---

## Сравнение OpenServer и Laragon

| Функция | OpenServer | Laragon |
|---------|------------|---------|
| **Лицензия** | Бесплатно (Donationware) | Бесплатно (Open Source) |
| **Интерфейс** | Русский | Английский |
| **Авто-виртуальные хосты** | Требуется настройка | Автоматически |
| **Смена версий PHP** | Через настройки | Быстрое переключение |
| **Терминал** | Нет | Встроенный |
| **Git** | Требуется установка | Встроен |
| **Composer** | Требуется установка | Встроен |
| **MailHog** | Нет | Встроен |
| **Портативность** | Частично | Полностью портативный |
| **Поддержка** | Форум, сообщество | GitHub, форум |

---

## Ссылки

- [Официальный сайт Laragon](https://laragon.org)
- [Документация Laragon](https://laragon.org/docs)
- [Документация WordPress](https://developer.wordpress.org)
- [GitHub Laragon](https://github.com/leokhoa/laragon)
- [Кодек WordPress (рус.)](https://wp-kama.com)
