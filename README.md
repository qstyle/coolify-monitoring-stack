# Coolify Logging Stack с FluentBit

Централизованная система логирования для Coolify с использованием **Loki** и **FluentBit**.

## 🚀 Быстрый старт

### 1. Развертывание в Coolify
- Создай новый сервис в Coolify
- Выбери тип "Docker Compose" 
- Скопируй содержимое `docker-compose.yml`
- Создай файлы конфигурации из папки `config/`

### 2. Проверка работы
```bash
# Проверь статус Loki
curl http://localhost:3100/ready

# Проверь доступные метки
curl http://localhost:3100/loki/api/v1/labels

# Получи последние логи
curl -G -s "http://localhost:3100/loki/api/v1/query_range" \
  --data-urlencode 'query={environment="coolify"}' \
  --data-urlencode 'start=1h' | jq
```

## 📊 Что собирается

### ✅ Автоматический сбор:
- **Логи всех Docker контейнеров** - все проекты Coolify
- **Системные логи** - Docker daemon, Coolify service
- **Структурированные логи** - JSON, nginx, PostgreSQL, MySQL
- **Метаданные контейнеров** - имена, образы, labels

### 🏷️ Автоматические метки:
- `environment=coolify` - все логи из Coolify
- `source=docker|systemd` - источник логов
- `container_name` - имя контейнера
- `image_name` - Docker образ
- `level` - уровень логирования (если распознан)

## 🔧 Архитектура

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Coolify App 1  │    │  Coolify App 2  │    │  Coolify App N  │
│   (Docker)      │    │   (Docker)      │    │   (Docker)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                        Docker Logs                             │
│                 /var/lib/docker/containers/                    │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │       FluentBit         │
                    │   (Сбор + обработка)   │
                    └─────────────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │         Loki            │
                    │    (Хранение логов)     │
                    └─────────────────────────┘
```

## 🔍 Запросы LogQL

### Основные запросы:
```logql
# Все логи Coolify
{environment="coolify"}

# Логи конкретного контейнера
{environment="coolify", container_name="my-app"}

# Ошибки за последний час
{environment="coolify"} |= "error" [1h]

# Логи по уровню (если распознан)
{environment="coolify", level="ERROR"}

# Nginx access логи
{environment="coolify", container_name=~".*nginx.*"} | json

# База данных логи
{environment="coolify", container_name=~".*postgres.*|.*mysql.*"}
```

### Продвинутые запросы:
```logql
# Топ контейнеров по ошибкам
topk(10, 
  count by (container_name) (
    {environment="coolify"} |= "error" [1h]
  )
)

# Статистика HTTP кодов
sum by (code) (
  count_over_time(
    {environment="coolify"} | json | code != "" [1h]
  )
)
```

## ⚙️ Конфигурация

### FluentBit особенности:
- **Продвинутый парсинг** - автоматическое распознавание форматов
- **Буферизация** - до 50MB в памяти
- **Фильтрация** - исключение системных полей
- **Обогащение** - добавление метаданных

### Loki особенности:
- **Локальное хранилище** - файлы в контейнере
- **Сжатие** - оптимизация места
- **Индексация** - по меткам для быстрого поиска
- **Ретеншн** - бессрочное хранение (можно настроить)

## 🐛 Диагностика

### Проверка FluentBit:
```bash
# Логи FluentBit
docker logs fluent-bit

# Статистика FluentBit
curl http://localhost:2020/api/v1/metrics/prometheus
```

### Проверка Loki:
```bash
# Health check
curl http://localhost:3100/ready

# Метрики
curl http://localhost:3100/metrics

# Конфигурация
curl http://localhost:3100/config
```

## 📈 Производительность

### Текущие лимиты:
- **Memory Buffer**: 50MB на FluentBit
- **Batch Size**: 1MB для отправки в Loki
- **Retention**: неограниченно (настраивается)

### Рекомендации:
- Для высокой нагрузки: увеличь `Mem_Buf_Limit`
- Для экономии места: настрой retention
- Для быстрого поиска: используй правильные метки

## 🔐 Безопасность

- Loki доступен только внутри Docker сети
- FluentBit имеет read-only доступ к логам
- Без аутентификации (добавь при необходимости)

## 📝 Дальнейшие улучшения

1. **Grafana** - веб-интерфейс для просмотра
2. **Alerting** - уведомления об ошибках  
3. **Retention** - автоматическая очистка старых логов
4. **TLS** - шифрование трафика
5. **Authentication** - защита доступа
