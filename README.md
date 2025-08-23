# 🚀 Coolify Monitoring Stack

Полноценный стек мониторинга для Coolify с Grafana, Prometheus, Loki и другими инструментами.

## ✨ Что включено

- **📊 Grafana** - красивые дашборды и визуализация
- **📈 Prometheus** - сбор и хранение метрик
- **📝 Loki** - агрегация и поиск логов
- **🔄 Promtail** - сбор логов с сервера
- **🖥️ Node Exporter** - метрики системы
- **🐳 cAdvisor** - метрики Docker контейнеров

## 🎯 Возможности

### 📊 Метрики
- **Системные ресурсы**: CPU, RAM, диск, сеть
- **Docker контейнеры**: использование ресурсов, статус
- **Coolify сервисы**: мониторинг развертываний
- **Автоматические алерты** при превышении лимитов

### 📝 Логирование
- **Централизованный сбор** всех логов
- **Поиск и фильтрация** по времени, сервису, уровню
- **Real-time логи** с обновлением каждые 10 секунд
- **Структурированные логи** с метаданными

### 🎨 Дашборды
- **System Overview** - общий обзор системы
- **Docker Containers** - мониторинг контейнеров
- **System Logs** - просмотр всех логов
- **Автоматическая загрузка** при запуске

## 🚀 Быстрый старт

### 1. Развертывание в Coolify

```bash
# Клонируйте репозиторий
git clone <your-repo>
cd coolify-monitoring-stack

# В Coolify укажите:
# - Source: Git Repository
# - Branch: main
# - Docker Compose файл: docker-compose.yml
# - Port: 3111 (Grafana)
```

### 2. Локальное развертывание

```bash
# Запустите все сервисы
docker-compose up -d

# Проверьте статус
docker-compose ps
```

## 🌐 Доступ к сервисам

После запуска будут доступны:

- **Grafana**: http://localhost:3111 (admin/admin123)
- **Prometheus**: http://localhost:9090
- **Loki**: http://localhost:3100
- **cAdvisor**: http://localhost:8081

## 📊 Первый вход в Grafana

1. **Откройте** http://localhost:3111
2. **Войдите** с учетными данными:
   - Username: `admin`
   - Password: `admin123`
3. **Дашборды загрузятся автоматически**

## 🔧 Настройка

### Переменные окружения

Создайте `.env` файл:

```bash
# Grafana
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=your_secure_password

# Prometheus
PROMETHEUS_RETENTION_TIME=200h

# Loki
LOKI_AUTH_ENABLED=false
```

### Пользовательские дашборды

Добавьте свои дашборды в:
```
monitoring/grafana/provisioning/dashboards/
```

### Дополнительные метрики

Настройте Prometheus в:
```
monitoring/prometheus/prometheus.yml
```

## 📈 Использование

### 1. Мониторинг системы

- **System Overview** дашборд показывает:
  - CPU, RAM, диск в реальном времени
  - Сетевой трафик
  - Статус сервисов

### 2. Мониторинг контейнеров

- **Docker Containers** дашборд отображает:
  - Использование ресурсов каждым контейнером
  - Сетевую активность
  - Общее количество контейнеров

### 3. Анализ логов

- **System Logs** дашборд позволяет:
  - Просматривать все логи в реальном времени
  - Фильтровать по ошибкам и предупреждениям
  - Искать по ключевым словам

### 4. Создание алертов

В Grafana настройте алерты:
- **CPU > 90%** - критическая нагрузка
- **RAM > 95%** - нехватка памяти
- **Disk > 90%** - мало места на диске

## 🔍 Troubleshooting

### Проблемы с портами

Если порт 3111 занят, измените в `docker-compose.yml`:
```yaml
ports:
  - "3112:3000"  # Используйте другой порт
```

### Дашборды не загружаются

Проверьте:
1. **Права доступа** к файлам
2. **Логи Grafana**: `docker logs grafana-monitoring`
3. **Источники данных** подключены

### Логи не собираются

Убедитесь:
1. **Promtail запущен**: `docker ps | grep promtail`
2. **Loki доступен**: `curl http://localhost:3100/ready`
3. **Права на чтение** логов

## 🚀 Расширение

### Добавление новых метрик

1. **Создайте exporter** для вашего сервиса
2. **Добавьте в Prometheus** конфигурацию
3. **Создайте дашборд** в Grafana

### Интеграция с внешними системами

- **Slack/Discord** уведомления
- **Email алерты**
- **Webhook интеграции**
- **API мониторинг**

### Масштабирование

Для production:
- **Prometheus** с удаленным хранилищем
- **Loki** с S3/PostgreSQL
- **Grafana** с аутентификацией
- **Load balancer** для высокой доступности

## 📚 Полезные ссылки

- [Grafana Documentation](https://grafana.com/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/)
- [Coolify Documentation](https://coolify.io/docs/)

## 🤝 Поддержка

Если у вас есть вопросы или проблемы:

1. **Проверьте логи**: `docker-compose logs [service-name]`
2. **Посмотрите статус**: `docker-compose ps`
3. **Перезапустите сервис**: `docker-compose restart [service-name]`

## 📄 Лицензия

MIT License - используйте свободно для любых целей.

---

**🎉 Теперь у вас есть профессиональный стек мониторинга для Coolify!**
