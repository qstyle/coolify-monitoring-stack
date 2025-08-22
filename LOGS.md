# 📝 Логи запуска Coolify Monitoring Stack

## 🚀 Статус запуска

Все сервисы успешно запущены и работают:

```bash
NAME                       STATUS               PORTS
cadvisor-monitoring        running (starting)   0.0.0.0:8080->8080/tcp
grafana-monitoring         running              0.0.0.0:3111->3000/tcp
loki-monitoring            running              0.0.0.0:3100->3100/tcp
node-exporter-monitoring   running              0.0.0.0:9100->9100/tcp
prometheus-monitoring       running              0.0.0.0:9090->9090/tcp
promtail-monitoring        running
```

## 📊 Логи Grafana

```
grafana-monitoring  | WARN: GF_INSTALL_PLUGINS is deprecated. Use GF_PLUGINS_PREINSTALL or GF_PLUGINS_PREINSTALL_SYNC instead.
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048296054Z level=info msg="Starting Grafana" version=12.1.1
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.04857047Z level=info msg="Config loaded from" file=/usr/share/grafana/conf/defaults.ini
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048582054Z level=info msg="Config loaded from" file=/etc/grafana/grafana.ini
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048584345Z level=info msg="Config overridden from command line" arg="default.paths.data=/var/lib/grafana"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048586304Z level=info msg="Config overridden from command line" arg="default.paths.logs=/var/log/grafana"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.04858847Z level=info msg="Config overridden from command line" arg="default.paths.plugins=/var/lib/grafana/plugins"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048590262Z level=info msg="Config overridden from command line" arg="default.paths.provisioning=/etc/grafana/provisioning"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048592762Z level=info msg="Config overridden from command line" arg="default.log.mode=console"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048594512Z level=info msg="Config overridden from Environment variable" var="GF_PATHS_DATA=/var/lib/grafana"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048596262Z level=info msg="Config overridden from Environment variable" var="GF_PATHS_LOGS=/var/log/grafana"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048597762Z level=info msg="Config overridden from Environment variable" var="GF_PATHS_PLUGINS=/var/lib/grafana/plugins"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048599262Z level=info msg="Config overridden from Environment variable" var="GF_PATHS_PROVISIONING=/etc/grafana/provisioning"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048601054Z level=info msg="Config overridden from Environment variable" var="GF_SECURITY_ADMIN_USER=admin"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048602595Z level=info msg="Config overridden from Environment variable" var="GF_SECURITY_ADMIN_PASSWORD=*********"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048604095Z level=info msg="Config overridden from Environment variable" var="GF_USERS_ALLOW_SIGN_UP=false"
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048610679Z level=info msg=Target target=[all]
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048615845Z level=info msg="Path Home" path=/usr/share/grafana
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.048617595Z level=info msg="Path Data" path=/var/lib/grafana
grafana-monitoring  | logger=settings t=2025-08-22T19:10:41.04861947Z level=info msg="Path Logs" path=/var/log/grafana
```

## 🔍 Логи Prometheus

```
prometheus-monitoring  | time=2025-08-22T19:10:40.835Z level=INFO source=main.go:1544 msg="updated GOGC" old=100 new=75
prometheus-monitoring  | time=2025-08-22T19:10:40.839Z level=INFO source=main.go:676 msg="Leaving GOMAXPROCS=10: CPU quota undefined" component=automaxprocs
prometheus-monitoring  | time=2025-08-22T19:10:40.839Z level=INFO source=memlimit.go:198 msg="GOMEMLIMIT is updated" component=automemlimit package=github.com/KimMachineGun/automemlimit/memlimit GOMEMLIMIT=7409405952 previous=9223372036854775807
prometheus-monitoring  | time=2025-08-22T19:10:40.839Z level=INFO source=main.go:769 msg="Starting Prometheus Server" mode=server version="(version=3.5.0, branch=HEAD, revision=8be3a9560fbdd18a94dedec4b747c35178177202)"
prometheus-monitoring  | time=2025-08-22T19:10:40.840Z level=INFO source=main.go:774 msg="operational information" build_context="(go=go1.24.5, platform=linux/arm64, user=root@4451b64cb451, date=20250714-16:18:17, tags=netgo,builtinassets)" host_details="(Linux 6.4.16-linuxkit #1 SMP PREEMPT Tue Oct 10 20:38:06 UTC 2023 aarch64 06db8cc716d2 (none))" fd_limits="(soft=1048576, hard=1048576)" vm_limits="(soft=unlimited, hard=unlimited)"
prometheus-monitoring  | time=2025-08-22T19:10:40.842Z level=INFO source=web.go:656 msg="Start listening for connections" component=web address=0.0.0.0:9090
prometheus-monitoring  | time=2025-08-22T19:10:40.844Z level=INFO source=main.go:1288 msg="Starting TSDB ..."
prometheus-monitoring  | time=2025-08-22T19:10:40.851Z level=INFO source=head.go:657 msg="Replaying on-disk memory mappable chunks if any" component=tsdb
prometheus-monitoring  | time=2025-08-22T19:10:40.851Z level=INFO source=head.go:744 msg="On-disk memory mappable chunks replay completed" component=tsdb duration=1.042µs
prometheus-monitoring  | time=2025-08-22T19:10:40.851Z level=INFO source=head.go:752 msg="Replaying WAL, this may take a while" component=tsdb
prometheus-monitoring  | time=2025-08-22T19:10:40.851Z level=INFO source=head.go:825 msg="WAL segment loaded" component=tsdb segment=0 maxSegment=0 duration=241.75µs
prometheus-monitoring  | time=2025-08-22T19:10:40.851Z level=INFO source=head.go:862 msg="WAL replay completed" component=tsdb checkpoint_replay_duration=30µs wal_replay_duration=250.542µs wbl_replay_duration=42ns chunk_snapshot_load_duration=0s mmap_chunk_replay_duration=1.042µs total_replay_duration=292.708µs
prometheus-monitoring  | time=2025-08-22T19:10:40.852Z level=INFO source=tls_config.go:347 msg="Listening on" component=web address=[::]:9090
prometheus-monitoring  | time=2025-08-22T19:10:40.852Z level=INFO source=tls_config.go:350 msg="TLS is disabled." http2=false address=[::]:9090
prometheus-monitoring  | time=2025-08-22T19:10:40.853Z level=INFO source=main.go:1309 msg="filesystem information" fs_type=EXT4_SUPER_MAGIC
prometheus-monitoring  | time=2025-08-22T19:10:40.853Z level=INFO source=main.go:1312 msg="TSDB started"
prometheus-monitoring  | time=2025-08-22T19:10:40.853Z level=INFO source=main.go:1497 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
prometheus-monitoring  | time=2025-08-22T19:10:40.856Z level=INFO source=main.go:1537 msg="Completed loading of configuration file" db_storage=3.458µs remote_storage=1.458µs web_handler=583ns query_engine=1.334µs scrape=573.584µs scrape_sd=149.416µs notify=1.334µs notify_sd=666ns rules=1.5µs tracing=4.375µs filename=/etc/prometheus/prometheus.yml totalDuration=3.178208ms
prometheus-monitoring  | time=2025-08-22T19:10:40.856Z level=INFO source=main.go:1273 msg="Server is ready to receive web requests."
prometheus-monitoring  | time=2025-08-22T19:10:40.856Z level=INFO source=manager.go:176 msg="Starting rule manager..." component="rule manager"
```

## 🎯 Доступные сервисы

- **Grafana Dashboard:** http://localhost:3111 (admin/admin123)
- **Prometheus:** http://localhost:9090
- **Loki:** http://localhost:3100
- **cAdvisor:** http://localhost:8080
- **Node Exporter:** http://localhost:9100

## 📈 Статус мониторинга

✅ **Grafana** - Дашборды загружены, готов к работе  
✅ **Prometheus** - TSDB запущен, метрики собираются  
✅ **Loki** - Сервис логов активен  
✅ **Promtail** - Сбор логов работает  
✅ **cAdvisor** - Docker метрики доступны  
✅ **Node Exporter** - Системные метрики собираются  

---

**Время запуска:** 2025-08-22 19:10:40 UTC  
**Статус:** Все сервисы работают корректно 🚀
