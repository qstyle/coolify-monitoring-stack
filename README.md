# 🚀 Coolify Monitoring Stack

Полноценный стек мониторинга для Coolify серверов с поддержкой метрик, логов и красивых дашбордов.

## ✨ Возможности

- 📊 **Метрики серверов** - CPU, RAM, диск, сеть
- 🐳 **Мониторинг Docker** - контейнеры, ресурсы, производительность
- 📝 **Централизованные логи** - все сервисы в одном месте
- 🎯 **Готовые дашборды** - красивый интерфейс для команды
- 🔔 **Алерты** - уведомления о проблемах
- 📈 **История** - анализ производительности во времени

## 🏗️ Архитектура

```
[Сервер 1 Coolify] ──┐
[Сервер 2 Coolify] ──┼──► [Promtail] ──► [Loki] ──► [Grafana]
[Сервер 3 Coolify] ──┘                    [Дашборды]

[Сервер 1 Coolify] ──┐
[Сервер 2 Coolify] ──┼──► [Node Exporter] ──► [Prometheus] ──► [Grafana]
[Сервер 3 Coolify] ──┘                                    [Дашборды]
```

## 🚀 Быстрый старт

### 1. Развертывание в Coolify (по умолчанию)
```bash
git clone <your-repo>
cd coolify-monitoring-stack
docker-compose up -d
```

**✅ Готово для Coolify:** Основной файл уже настроен для развертывания в Coolify!

### 2. Локальное развертывание
```bash
# Для локального использования используйте специальный файл
docker-compose -f docker-compose.local.yml up -d
```

### 3. Доступ к сервисам
- **Grafana:** http://localhost:3111 (admin/admin123)
- **Prometheus:** http://localhost:9090
- **Loki:** http://localhost:3100
- **cAdvisor:** http://localhost:8081 (Coolify) / http://localhost:8080 (локально)

### 3. Первый вход в Grafana
1. Откройте http://localhost:3000
2. Логин: `admin`
3. Пароль: `admin123`
4. Дашборды загрузятся автоматически

## 🔧 Настройка для продакшна

### Добавление серверов Coolify

1. **Установите Node Exporter на каждый сервер:**
```bash
# На сервере Coolify
curl -fsSL https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz | tar xvz
sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

2. **Обновите prometheus.yml:**
```yaml
- job_name: 'coolify-servers'
  static_configs:
    - targets: 
      - '192.168.1.10:9100'  # Сервер 1
      - '192.168.1.11:9100'  # Сервер 2
      - '192.168.1.12:9100'  # Сервер 3
```

3. **Перезапустите Prometheus:**
```bash
docker-compose restart prometheus
```

### Настройка NestJS приложений

1. **Установите пакеты:**
```bash
npm install winston winston-loki @willsoto/nestjs-prometheus prom-client
```

2. **Настройте логирование:**
```typescript
// winston.config.ts
import { WinstonModule } from 'nest-winston';
import * as winston from 'winston';
import LokiTransport from 'winston-loki';

export const winstonConfig = WinstonModule.createLogger({
  transports: [
    new winston.transports.Console(),
    new LokiTransport({
      host: "http://your-loki-server:3100",
      json: true,
      labels: { job: 'nestjs-app' },
      format: winston.format.json(),
      replaceTimestamp: true
    })
  ]
});
```

3. **Настройте метрики:**
```typescript
// app.module.ts
import { PrometheusModule } from '@willsoto/nestjs-prometheus';

@Module({
  imports: [
    PrometheusModule.register({
      path: '/metrics',
      defaultMetrics: { enabled: true }
    })
  ]
})
export class AppModule {}
```

## 📊 Доступные дашборды

### System Overview
- CPU, RAM, диск, сеть
- Системные метрики в реальном времени
- Цветовая индикация проблем

### Docker Containers
- Использование ресурсов контейнерами
- Сетевой трафик
- Статус контейнеров

### Логи (через Explore)
- Поиск по всем сервисам
- Фильтрация по меткам
- Анализ ошибок

## 🔒 Безопасность

### Для продакшна рекомендуется:
1. **HTTPS** для всех внешних URL
2. **Аутентификация** в Grafana
3. **Firewall** для внутренних портов
4. **Переменные окружения** для паролей

### Создание .env файла:
```bash
# .env
GF_SECURITY_ADMIN_PASSWORD=your_secure_password
```

## 🚨 Алерты

### Настройка уведомлений:
1. **В Grafana** → Alerting → Notification channels
2. **Добавьте** Slack, Telegram, Email
3. **Создайте** правила алертов
4. **Настройте** пороги для CPU, RAM, диска

### Пример алерта:
```yaml
# CPU > 90% на 5 минут
expr: 100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 90
for: 5m
```

## 📈 Масштабирование

### Горизонтальное масштабирование:
1. **Добавляйте серверы** в prometheus.yml
2. **Настройте балансировку** для Grafana
3. **Используйте внешние** базы данных для Prometheus/Loki

### Вертикальное масштабирование:
1. **Увеличьте ресурсы** контейнеров
2. **Настройте retention** для метрик
3. **Оптимизируйте** запросы

## 🆘 Устранение неполадок

### Prometheus не собирает метрики:
```bash
# Проверьте статус
docker-compose ps prometheus

# Посмотрите логи
docker-compose logs prometheus

# Проверьте конфигурацию
curl http://localhost:9090/api/v1/targets
```

### Loki не принимает логи:
```bash
# Проверьте статус
docker-compose ps loki

# Посмотрите логи
docker-compose logs loki

# Проверьте Promtail
docker-compose logs promtail
```

### Grafana не загружается:
```bash
# Проверьте статус
docker-compose ps grafana

# Посмотрите логи
docker-compose logs grafana

# Проверьте volumes
docker volume ls
```

## 🤝 Поддержка

- **Issues:** Создавайте в GitHub
- **Discussions:** Обсуждайте в GitHub Discussions
- **Wiki:** Дополнительная документация

## 📄 Лицензия

MIT License - используйте свободно!

---

**Создано с ❤️ для сообщества Coolify**
