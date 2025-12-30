# Тестовое задание DevOps — Effective Mobile

## Как запустить
#### Установите Docker и Docker Compose и клонируйте репозиторий:

```bash
git clone https://github.com/man-from-podolsk/effective-mobile-devops.git
cd effective-mobile-devops
```

#### Соберите и запустите приложение:
```bash
docker compose up --build -d
```
## Как проверить
#### Выполните в терминале:

```bash
curl http://localhost
```

#### Ожидаемый результат:

```bash
Hello from Effective Mobile!
```

#### Проверьте, что бэкенд недоступен напрямую:

```bash
curl http://localhost:8080
```

> (Должна возникнуть ошибка подключения)

```bash
curl: (7) Failed to connect to localhost port 8080 after 3 ms: Could not connect to server
```

## Архитектура приложения

| Компонент       | Порт/Сеть                | Назначение                                 |
|-----------------|--------------------------|-------------------------------------------|
| Пользователь    | 80 (хост)                | Внешний доступ к приложению               |
| Nginx           | 80 (внутри контейнера)   | Единая точка входа, проксирует запросы    |
| Backend         | 8080 (внутри Docker-сети)| Обработка запросов через Python http.server |

Nginx — единственная точка входа, публикует порт 80 на хост, Backend работает на Python http.server, доступен только внутри Docker-сети.

Все сервисы взаимодействуют через изолированную сеть app-network

## Используемые технологии
  - Python 3.11 (встроенный http.server)
  - Nginx (официальный образ)
  - Docker и Docker Compose

## Безопасность
  - Backend запускается от непривилегированного пользователя appuser
  - Порт 8080 бэкенда не опубликован наружу
  - Используется изолированная Docker-сеть
  - Нет хранения секретов в репозитории

## Проверка работоспособности
#### Дополнительные команды для проверки:
Проверить статус контейнеров
```bash
docker compose ps
```
Посмотреть логи Nginx
```bash
docker logs effective-nginx
```
Посмотреть логи Backend
```bash
docker logs effective-backend
```

 > Примечание: Для остановки приложения используйте:
```bash
docker compose down
```
