##НАстройка интеграции с OpenTelemetry Signoz

#Необходимо открыть файл setenv.sh и вставить следующие настройки: </br>

``
OTEL_AGENT_PATH="/etc/opentelemetry/opentelemetry-javaagent.jar"
export OTEL_RESOURCE_ATTRIBUTES="service.name=nameserver"
export OTEL_EXPORTER_OTLP_ENDPOINT="http://nameserver:4318"
export OTEL_METRICS_EXPORTER="none"
export OTEL_LOGS_EXPORTER="none"
#export OTEL_EXPORTER_OTLP_CERTIFICATE="/etc/opentelemetry/signoz-stage.crt"
export JAVA_OPTS="-javaagent:$OTEL_AGENT_PATH $JAVA_OPTS"
``

После этого необходимо перезагрузить систему.

ДАльше рассмотрим интеграцию с ELK.