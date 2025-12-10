# MT-01 — MANUAL TÉCNICO DE LOGGING  
**Manual Técnico de Nivel 2**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**  
Vinculado a: **SP-01 Política de Logging**

---

## 1. Propósito Operativo
Definir los estándares técnicos, configuraciones mínimas, estructuras de mensajes, mecanismos de correlación y lineamientos prácticos para implementar logging consistente, seguro, estructurado y auditable en todos los sistemas administrados por la Gerencia TICs.

Este manual **traduce la norma (SP-01)** en instrucciones técnicas concretas para desarrolladores, QA, arquitectos y equipos de operación.

---

## 2. Requerimientos Fundamentales del Logging

### 2.1 Formato Estructurado (Obligatorio)
Todos los logs deben generarse en formato **JSON** con claves estandarizadas:

| Campo | Descripción |
|-------|-------------|
| `timestamp` | Fecha completa en formato ISO-8601 |
| `level` | Nivel de severidad (DEBUG, INFO, WARN, ERROR, FATAL) |
| `service` | Nombre único del servicio |
| `environment` | Ambiente: dev, qa, uat, preprod, prod |
| `correlation_id` | Identificador único transversal |
| `operation` | Nombre de la operación o endpoint |
| `message` | Mensaje principal |
| `duration_ms` | Tiempo de ejecución (si aplica) |
| `exception` | Stacktrace (solo en errores) |

### 2.2 Timestamps
- Siempre en **UTC**.  
- Formato recomendado: `YYYY-MM-DDTHH:mm:ss.sssZ`

### 2.3 Correlation-ID
- Obligatorio en todas las llamadas entrantes y salientes.  
- Debe transmitirse entre microservicios mediante encabezado HTTP:
```
X-Correlation-ID: <uuid>
```


### 2.4 Restricciones de Seguridad
Prohibido registrar:
- Contraseñas  
- Tokens  
- Claves privadas  
- Secretos  
- Información sensible (PII), salvo autorización formal de Seguridad

El validador in-house debe bloquear automáticamente commits con datos sensibles.

---

## 3. Estructura Estándar del Log

Ejemplo en formato JSON:

```
{
  "timestamp": "2025-01-01T12:00:35.125Z",
  "level": "INFO",
  "service": "clientes-api",
  "environment": "prod",
  "correlation_id": "d23f8aab-11e8-4fb0-a32a-99e10b445abf",
  "operation": "GET /clientes/123",
  "message": "Consulta de cliente realizada exitosamente",
  "duration_ms": 42
}
```

---

## 4. Lineamientos Detallados

### 4.1 Logging en entradas y salidas
En cada request debe registrarse:
- Inicio de operación  
- Fin de operación  
- Duración  
- Correlation-ID  
- Resultado (éxito o error)

### 4.2 Logging de errores
Todo error debe incluir:

```
{
  "level": "ERROR",
  "exception": "java.lang.IllegalArgumentException: ...",
  "stacktrace": "...",
  "correlation_id": "...",
  "operation": "POST /pago"
}
```

### 4.3 Niveles de Logging — Reglas de Uso

| Nivel | Uso |
|------|-----|
| DEBUG | Solo en DEV/QA |
| INFO | Flujo normal |
| WARN | Situaciones no críticas |
| ERROR | Fallos funcionales/técnicos |
| FATAL | Indisponibilidad total del servicio |

---

## 5. Integración con Monitoreo

El logging debe enviarse automáticamente a la plataforma de observabilidad mediante:
- Fluentd  
- Filebeat  
- OpenTelemetry  
- Agentes corporativos aprobados  

Formato estándar de envío:
- JSON line por línea  
- Sin compresión en origen  
- TLS obligatorio

---

## 6. Configuraciones Mínimas por Tecnología

### 6.1 Python (logging + structlog)
```
import structlog

structlog.configure(
    processors=[
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.JSONRenderer()
    ]
)
```

### 6.2 Java (Logback)
Logback.xml recomendado:

```
<encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
  <providers>
    <timestamp/>
    <version/>
    <loggerName/>
    <message/>
    <arguments/>
  </providers>
</encoder>
```

### 6.3 Node.js (pino)
```
const pino = require('pino');
const logger = pino({ level: 'info' });
```

---

## 7. Buenas Prácticas

- Mantener mensajes cortos, precisos y sin tecnicismos irrelevantes.  
- No registrar objetos grandes (buffers, blobs, imágenes).  
- Evitar logging excesivo (log spam).  
- Asegurar la existencia de dashboards basados en logs.  
- Usar listas blancas de campos permitidos para evitar filtración de datos.

---

## 8. Advertencias y Errores Frecuentes
- ❌ Registrar datos sensibles → **violación grave de seguridad**  
- ❌ No enviar logs al sistema centralizado → incumplimiento de SP-02  
- ❌ No incluir correlation-id → afecta trazabilidad  
- ❌ Logs en texto plano → incumplimiento de SP-01  
- ❌ Logging excesivo → afecta latencia y costos de almacenamiento  

---

## 9. Requisitos Obligatorios en CI/CD
El pipeline debe validar:
- Presencia de logs estructurados  
- Correlation-ID funcional  
- Formato JSON válido  
- Ausencia de secretos (validador in-house)  
- Logs visibles en la plataforma de monitoreo

---

## 10. Anexos

### Anexo A — Plantilla de Logging
```
{
  "timestamp": "",
  "level": "",
  "service": "",
  "environment": "",
  "correlation_id": "",
  "operation": "",
  "message": "",
  "duration_ms": 0,
  "exception": null
}
```

### Anexo B — Lista de Campos Prohibidos
- password  
- token  
- api_key  
- private_key  
- session_identifier  
- sensitive_personal_data  

---

# FIN DEL MANUAL MT-01
