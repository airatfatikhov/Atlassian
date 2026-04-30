Оптимизация кэширования в Jira Data Center (DC) требует системного подхода: Jira DC использует многоуровневую архитектуру кэшей, и слепое увеличение объёмов часто даёт обратный эффект (OOM, паузы GC, фрагментация Hazelcast). Ниже приведён практический гайд по диагностике и оптимизации, основанный на рекомендациях Atlassian и производственной практике.

## ✅ Hazelcast (распределённый кэш)

 - В cluster.properties убедитесь в использовании TCP-IP вместо multicast в production

 - Задайте явные адреса узлов:

````
hazelcast.multicast.enabled=false
hazelcast.tcpip.enabled=true
hazelcast.tcpip.members=10.0.1.10,10.0.1.11,10.0.1.12
````

## ✅ Локальные кэши & jira-config.properties

Ручная настройка кэшей рекомендуется только после profiling. Пример безопасных твиков:

````
# Увеличить TTL для редко меняющихся данных (в секундах)
jira.cache.ttl.default=3600
# Отключить кэширование тяжёлых запросов, если они вызывают thrashing
jira.cache.issue.search.enabled=false
````