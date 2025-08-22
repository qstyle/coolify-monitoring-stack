# 🚀 Развертывание в Coolify

## ⚠️ Проблема с портами

При развертывании в Coolify может возникнуть конфликт портов, особенно с портом 8080, который часто занят другими сервисами.

## 🔧 Решение

Используйте файл `docker-compose.coolify.yml` вместо стандартного `docker-compose.yml`.

### Основные изменения:

- **cAdvisor порт изменен** с `8080:8080` на `8081:8080`
- **Убраны container_name** для автоматического именования Coolify
- **Добавлены labels** для управления Coolify

## 📋 Пошаговое развертывание

### 1. Загрузите проект в Coolify

```bash
# Клонируйте репозиторий
git clone https://github.com/qstyle/coolify-monitoring-stack.git
cd coolify-monitoring-stack
```

### 2. Создайте новый сервис в Coolify

- **Источник:** Git Repository
- **Ветка:** main
- **Docker Compose файл:** `docker-compose.coolify.yml`
- **Порт:** 3111 (Grafana)

### 3. Настройте переменные окружения (опционально)

```bash
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=your_secure_password
```

### 4. Запустите развертывание

Coolify автоматически:
- Создаст контейнеры
- Настроит сеть
- Запустит все сервисы

## 🌐 Доступные порты

После развертывания:

- **Grafana:** http://your-server:3111 (admin/admin123)
- **Prometheus:** http://your-server:9090
- **Loki:** http://your-server:3100
- **cAdvisor:** http://your-server:8081 ⚠️ **ИЗМЕНЕН**
- **Node Exporter:** http://your-server:9100

## 🔍 Проверка работы

### 1. Проверьте статус контейнеров

```bash
# В Coolify Dashboard
# Или через SSH на сервере
docker ps | grep monitoring
```

### 2. Проверьте логи

```bash
# В Coolify Dashboard
# Или через SSH
docker-compose -f docker-compose.coolify.yml logs
```

### 3. Отправьте тестовые логи

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"streams": [{"stream": {"app": "coolify-app", "level": "info"}, "values": [["'$(date +%s)000000000'", "Тест развертывания в Coolify"]]}]}' \
  http://your-server:3100/loki/api/v1/push
```

## 🚨 Устранение неполадок

### Порт 8080 занят

**Симптом:** `Bind for 0.0.0.0:8080 failed: port is already allocated`

**Решение:** Используйте `docker-compose.coolify.yml` с портом 8081

### Контейнеры не запускаются

**Проверьте:**
1. Достаточно ли ресурсов на сервере
2. Нет ли конфликтов с другими сервисами
3. Правильно ли настроены volumes и networks

### Логи не отображаются

**Проверьте:**
1. Статус Loki контейнера
2. Подключение Grafana к Loki
3. Временной диапазон в дашборде

## 📊 Мониторинг

После успешного развертывания у вас будет:

- ✅ **Полный стек мониторинга** для Coolify серверов
- ✅ **Централизованные логи** через Loki
- ✅ **Метрики системы** через Prometheus
- ✅ **Красивые дашборды** в Grafana
- ✅ **Автоматическое обновление** каждые 5 секунд

## 🔄 Обновления

Для обновления:

1. **Pull изменений** в репозитории
2. **Перезапустите сервис** в Coolify
3. **Проверьте логи** на ошибки

---

**Успешного развертывания! 🎉**
