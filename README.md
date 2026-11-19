# Игра «Змейка»

Статическое веб-приложение упаковывается в Docker-образ на базе `nginx:alpine`.
Отдельный этап компиляции не требуется: Docker копирует готовый файл `index.html`
в каталог с HTML-файлами Nginx.

## Предварительные требования

- Docker Desktop с включенным Linux container engine.
- Docker Compose (обычно входит в Docker Desktop).

Из корня проекта проверьте доступ к демону Docker:

```powershell
docker --context desktop-linux version
```

Если команда использует устаревшую переменную окружения `DOCKER_HOST`, очистите её:

```powershell
Remove-Item Env:DOCKER_HOST -ErrorAction SilentlyContinue
docker context use desktop-linux
```

## Сборка образа

```powershell
cd D:\Projects\Vibecoding\vps_coolify
docker --context desktop-linux build -t snake-game:latest .
```

Проверить созданный образ:

```powershell
docker --context desktop-linux images snake-game
```

## Запуск контейнера

Запустите контейнер в фоновом режиме и опубликуйте порт 8080:

```powershell
docker --context desktop-linux run --detach --name snake-game --publish 8080:80 --restart unless-stopped snake-game:latest
```

Приложение будет доступно по адресу http://localhost:8080.

Проверить журнал и состояние контейнера:

```powershell
docker --context desktop-linux ps
docker --context desktop-linux logs snake-game
```

## Запуск через Docker Compose

```powershell
docker-compose up --detach --build
```

Compose использует [docker-compose.yml](docker-compose.yml) и собирает образ из
текущего каталога. Порт 80 доступен контейнерам и прокси Coolify, но не
публикуется непосредственно на сервере. В Coolify настройте домен приложения
на сервис `snake` и его внутренний порт `80`.

Для локальной проверки откройте приложение на `http://localhost:8080`, запустив
контейнер командой `docker run` из раздела выше.

Остановить и удалить контейнеры Compose:

```powershell
docker-compose down
```

## Остановка контейнера, созданного командой `docker run`

```powershell
docker --context desktop-linux stop snake-game
docker --context desktop-linux rm snake-game
```
