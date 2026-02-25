# Настройка WordPress на OpenServer

## Содержание

1. [Требования](#требования)
2. [Установка OpenServer](#установка-openserver)
3. [Настройка проекта](#настройка-проекта)
4. [Создание базы данных](#создание-базы-данных)
5. [Установка WordPress](#установка-wordpress)
6. [Настройка wp-config.php](#настройка-wp-configphp)
7. [Дополнительные настройки](#дополнительные-настройки)
8. [Устранение проблем](#устранение-проблем)

---

## Требования

- **ОС:** Windows 10/11 (64-bit)
- **OpenServer Panel:** версия 6.x или новее
- **PHP:** 8.1 или выше (рекомендуется 8.4)
- **MySQL/MariaDB:** 5.7+ или 10.3+
- **Свободное место:** минимум 500 МБ

---

## Установка OpenServer

### Шаг 1: Скачивание

1. Перейдите на официальный сайт: [https://ospanel.io](https://ospanel.io)
2. Скачайте последнюю версию OpenServer Panel
3. Распакуйте архив в удобную папку (например, `C:\OSPanel`)

### Шаг 2: Первый запуск

1. Запустите `OSPanel.exe` от имени администратора
2. Дождитесь загрузки модулей (PHP, MySQL, Apache/Nginx)
3. В трее появится иконка OpenServer (флаг)

### Шаг 3: Настройка модулей

1. Кликните по иконке в трее → **Настройки**
2. Перейдите во вкладку **Модули**
3. Выберите версии:
   - **PHP:** 8.4 (или 8.1+)
   - **MySQL:** MariaDB 10.6+ или MySQL 8.0+
   - **HTTP-движок:** Caddy или Nginx
4. Нажмите **Сохранить** и перезапустите OpenServer

---

## Настройка проекта

### Шаг 1: Создание домена

1. В папке `domains` (или `home`) создайте папку с именем домена:
   ```
   M:\OSPanel\home\wordpress-6.9.1.local\
   ```

2. Скопируйте файлы WordPress в эту папку

### Шаг 2: Конфигурация проекта

В папке проекта создайте файл `.osp\project.ini`:

```ini
[wordpress-6.9.1.local]

app_start_command =
backend_enabled   = off
backend_ip        = auto
backend_port      = auto
base_url          = {host_scheme}://{host_decoded}{scheme_port}
bind_ip           = auto
environment       = System
http_engine       = Caddy
http_port         = 80
https_port        = 443
node_engine       = auto
php_engine        = PHP-8.4
primary_domain    = off
project_category  =
project_enabled   = on
project_root      = {base_dir}
server_aliases    = www.{host}
terminal_codepage = 65001
tls_cert_file     = auto
tls_enabled       = on
tls_key_file      = auto
web_root          = {base_dir}
```

### Шаг 3: Активация проекта

1. Кликните по иконке OpenServer в трее
2. Выберите **Мои проекты**
3. Найдите `wordpress-6.9.1.local` и убедитесь, что он включён
4. Перезапустите OpenServer

---

## Создание базы данных

### Способ 1: Через phpMyAdmin

1. Откройте меню OpenServer → **Дополнительно** → **phpMyAdmin**
2. Авторизуйтесь (по умолчанию: пользователь `root`, пароль пустой)
3. Нажмите вкладку **Базы данных**
4. Введите имя базы: `wordpress`
5. Выберите кодировку: `utf8mb4_unicode_ci`
6. Нажмите **Создать**

### Способ 2: Через консоль MySQL

1. Откройте меню OpenServer → **Дополнительно** → **Консоль MySQL**
2. Выполните команды:

```sql
CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
SHOW DATABASES;
EXIT;
```

### Проверка доступа

| Параметр | Значение по умолчанию |
|----------|----------------------|
| Хост | `localhost` |
| Порт | `3306` |
| Пользователь | `root` |
| Пароль | (пустой) или `root` |

---

## Установка WordPress

### Шаг 1: Подготовка

1. Убедитесь, что файл `wp-config.php` существует и настроен
2. Проверьте, что база данных создана

### Шаг 2: Запуск установщика

1. Откройте браузер
2. Перейдите по адресу: `https://wordpress-6.9.1.local`
3. Выберите язык установки (русский)
4. Нажмите **Продолжить**

### Шаг 3: Заполнение данных

Введите информацию о сайте:

- **Название сайта:** Например, "Мой сайт на WordPress"
- **Имя пользователя:** admin (или другое)
- **Пароль:** Надёжный пароль (минимум 12 символов)
- **Email:** Ваш email
- **Поисковые системы:** Галочка "Рекомендовать поисковым системам не индексировать сайт" (для локальной разработки)

### Шаг 4: Завершение

1. Нажмите **Установить WordPress**
2. После успешной установки нажмите **Войти**
3. Авторизуйтесь под созданным администратором

---

## Настройка wp-config.php

### Базовая конфигурация для OpenServer

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

Используйте официальный генератор: [https://api.wordpress.org/secret-key/1.1/salt/](https://api.wordpress.org/secret-key/1.1/salt/)

Скопируйте полученные ключи и вставьте в `wp-config.php`

---

## Дополнительные настройки

### Включение отладки

Для разработки включите режим отладки:

```php
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
define( 'SCRIPT_DEBUG', true );
```

Логи будут сохраняться в `wp-content/debug.log`

### Увеличение лимитов PHP

Создайте файл `.user.ini` в корне WordPress:

```ini
memory_limit = 256M
max_execution_time = 300
upload_max_filesize = 64M
post_max_size = 64M
```

Или измените настройки в меню OpenServer → **Настройки** → **Модули** → **PHP**

### Настройка постоянных ссылок

1. В админ-панели WordPress перейдите: **Настройки** → **Постоянные ссылки**
2. Выберите структуру: `Название записи`
3. Сохраните изменения

### Включение HTTPS

OpenServer автоматически генерирует самоподписанные SSL-сертификаты. При первом заходе браузер может показать предупреждение — нажмите "Продолжить" или "Принять риск".

---

## Устранение проблем

### Ошибка подключения к базе данных

**Проблема:** `Error establishing a database connection`

**Решения:**
1. Проверьте, запущен ли MySQL в OpenServer
2. Убедитесь, что БД создана
3. Проверьте учётные данные в `wp-config.php`
4. Попробуйте заменить `localhost` на `127.0.0.1`

### Белый экран смерти

**Проблема:** Пустой белый экран

**Решения:**
1. Включите отладку в `wp-config.php`:
   ```php
   define( 'WP_DEBUG', true );
   define( 'WP_DEBUG_DISPLAY', true );
   ```
2. Проверьте логи в `wp-content/debug.log`
3. Увеличьте лимит памяти PHP
4. Отключите все плагины (переименуйте папку `plugins`)

### Ошибка 404 на страницах

**Проблема:** Страницы кроме главной возвращают 404

**Решения:**
1. Пересохраните постоянные ссылки в админке
2. Проверьте наличие `.htaccess` (для Apache)
3. Убедитесь, что mod_rewrite включен

### Проблемы с загрузкой файлов

**Проблема:** Не удаётся загрузить медиафайлы

**Решения:**
1. Проверьте права на папку `wp-content/uploads`
2. Увеличьте `upload_max_filesize` и `post_max_size`
3. Убедитесь, что папка `uploads` существует

### Медленная работа

**Решения:**
1. Включите кэширование OPcache в настройках PHP
2. Установите плагин кэширования (WP Super Cache, W3 Total Cache)
3. Отключите лишние плагины
4. Увеличьте `memory_limit` до 512M

---

## Полезные команды

### Перезапуск OpenServer через консоль

```powershell
# Остановка
taskkill /F /IM OSPanel.exe

# Запуск (от администратора)
"C:\OSPanel\OSPanel.exe"
```

### Сброс кэша OPcache

1. Меню OpenServer → **Дополнительно** → **Перезапуск PHP**

### Просмотр логов MySQL

```
M:\OSPanel\userdata\MySQL\mysql_error.log
```

### Логи WordPress

```
M:\OSPanel\home\wordpress-6.9.1.local\wp-content\debug.log
```

---

## Ссылки

- [Официальный сайт OpenServer](https://ospanel.io)
- [Документация WordPress](https://developer.wordpress.org)
- [Форум поддержки OpenServer](https://ospanel.io/forum)
- [Кодек WordPress (рус.)](https://wp-kama.com)
