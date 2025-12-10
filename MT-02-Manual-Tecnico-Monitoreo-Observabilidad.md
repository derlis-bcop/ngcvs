# MT-02 — MANUAL TÉCNICO DE MONITOREO Y OBSERVABILIDAD  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-02 Política de Monitoreo y Observabilidad**

---

# 1. Propósito Operativo
Este manual define los estándares técnicos, configuraciones mínimas, endpoints, métricas, trazas, integraciones y procedimientos necesarios para implementar monitoreo y observabilidad completos en todos los sistemas administrados por la Gerencia TICs.

Su objetivo es transformar las obligaciones normativas de SP-02 en pasos concretos para desarrolladores, QA, DevOps, Producción e Infraestructura.

---

# 2. Componentes Fundamentales de la Observabilidad

La observabilidad institucional se compone de tres pilares:

## 2.1 Logs (ya normados en SP-01)
- Formato estructurado JSON  
- Trazabilidad mediante correlation-id  
- Envío a plataforma unificada  

## 2.2 Métricas (obligatorias)
Tipos:
- **Técnicas:** CPU, memoria, disco, IO, threads, conexiones  
- **Aplicativas:** latencia, throughput, tasa de éxito/error, colas pendientes  
- **Negocio (funcionales):** métricas definidas por producto (cubiertas en SP-17 futura)

## 2.3 Trazas (Traces)
Obligatorias para:
- Microservicios  
- APIs  
- Integraciones  
- Procesos distribuidos

Se implementan mediante OpenTelemetry o agente equivalente.

---

# 3. Estándares Técnicos de Métricas

## 3.1 Recolección obligatoria
Toda aplicación debe exponer:
- `/metrics` en formato Prometheus  
- Métricas propias + métricas estándar del runtime  

## 3.2 Métricas mínimas obligatorias
| Tipo | Métrica | Descripción |
|------|---------|-------------|
| Performance | `http_server_requests_seconds_*` | Latencia por endpoint |
| Disponibilidad | `up` | Si el servicio está operativo |
| Errores | `http_server_errors_total` | Cantidad por tipo |
| Recursos | `process_cpu_usage`, `process_memory_bytes` | Estado del host |
| Uso | `requests_total` | Cantidad de peticiones procesadas |

## 3.3 Formato de Métricas Prometheus Ejemplo
```
# HELP http_server_requests_seconds Latencia de operaciones
# TYPE http_server_requests_seconds histogram
http_server_requests_seconds_bucket{le="0.1"} 25
http_server_requests_seconds_bucket{le="0.2"} 40
http_server_requests_seconds_sum 12.4
http_server_requests_seconds_count 82
```

---

# 4. Trazas Distribuidas (Tracing)

## 4.1 Obligación de Instrumentación
Toda aplicación debe:
- Generar spans por operación  
- Propagar `traceparent` y `tracestate`  
- Incluir correlation-id en cada span  

## 4.2 Ejemplo de Instrumentación OpenTelemetry (Python)
```
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor

FlaskInstrumentor().instrument_app(app)
tracer = trace.get_tracer(__name__)
```

## 4.3 Ejemplo en Node.js
```
const { NodeSDK } = require('@opentelemetry/sdk-node');
const sdk = new NodeSDK();
sdk.start();
```

---

# 5. Health Checks — Integración con el Monitoreo

Los endpoints `/health`, `/live`, `/ready` definidos en SP-05 deben integrarse con el monitoreo:

- Estado `UP / DOWN / DEGRADED`  
- Nombre del servicio  
- Dependencias evaluadas  
- Tiempo de respuesta  

Ejemplo:

```
{
  "status": "UP",
  "checks": {
    "db": "UP",
    "cache": "UP"
  }
}
```

---

# 6. Dashboards Estándar por Servicio

Cada servicio debe contar con al menos 3 dashboards:

## 6.1 Dashboard de Performance
- Latencia p50, p90, p99  
- Throughput  
- Errores por minuto  

## 6.2 Dashboard de Recursos
- CPU  
- Memoria  
- IO  
- Conexiones activas  

## 6.3 Dashboard de Disponibilidad
- SLO vs Real  
- Eventos DOWN  
- Alertas activadas  
- Readiness durante despliegues  

---

# 7. Alertamiento

## 7.1 Reglas mínimas obligatorias
- Estado DOWN → alerta crítica  
- Latencia > umbral p99 → alerta alta  
- Errores > 5% en 5 minutos → alerta alta  
- Degradación recurrente → alerta media  

## 7.2 Modelo de Alertas con Prometheus Alertmanager
```
groups:
- name: servicio-alertas
  rules:
  - alert: ServicioCaido
    expr: up == 0
    for: 2m
    labels:
      severity: critical
    annotations:
      message: "El servicio {{ $labels.job }} está caído."
```

---

# 8. Integración con Plataforma Institucional de Observabilidad

Producción/Infraestructura debe proveer:
- Endpoints de ingestión  
- Agentes certificados  
- Conectores para logs, métricas y trazas  
- Dashboards base  
- Alerta preconfiguradas  

Aplicaciones NO deben enviar telemetría directamente a internet.

---

# 9. Configuraciones Mínimas por Tecnología

## 9.1 Java (Spring Boot)
Habilitar Actuator + Micrometer:

```
management.endpoints.web.exposure.include=health,info,prometheus
management.metrics.export.prometheus.enabled=true
```

## 9.2 Node.js (express + prom-client)
```
const client = require('prom-client');
client.collectDefaultMetrics();
```

## 9.3 Python (FastAPI + prometheus_client)
```
from prometheus_client import start_http_server
start_http_server(8001)
```

---

# 10. Pruebas y Validaciones en QA

QA debe validar:
- Métricas exponiéndose correctamente  
- Alertas disparándose bajo condiciones simuladas  
- Trazas visibles en la plataforma  
- Dashboards funcionando  
- Logs correlacionados con trazas en incidentes

---

# 11. Buenas Prácticas

- Mantener bajo volumen de métricas personalizadas  
- Evitar series cardinales infinitas (ej: métricas por ID de usuario)  
- Minimizar costos de almacenamiento  
- Documentar cada métrica expuesta  
- No incluir lógica de negocio en health checks  

---

# 12. Advertencias Comunes

- ❌ No exponer `/metrics` → incumplimiento crítico  
- ❌ Métricas con etiquetas dinámicas infinitas → inutiliza Prometheus  
- ❌ Falta de trazas → diagnóstico difícil  
- ❌ No centralizar monitoreo → incumplimiento de SP-02  
- ❌ Falta de dashboards → análisis incompleto  

---

# 13. Requisitos Obligatorios en CI/CD

Cada pipeline debe verificar:
- Exposición del endpoint `/metrics`  
- Instrumentación OpenTelemetry activa  
- Validación de health checks  
- Existencia de dashboards mínimos  
- Alarmas configuradas  

---

# FIN DEL MANUAL MT-02
