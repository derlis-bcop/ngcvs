# MT-05 — MANUAL TÉCNICO DE HEALTH CHECKS Y DISPONIBILIDAD  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-05 Política de Health Checks y Disponibilidad**

---

# 1. Propósito Operativo
Este manual establece las instrucciones técnicas para implementar mecanismos de verificación de estado (health checks) que permitan:

- Identificar fallos rápidamente  
- Verificar readiness durante despliegues  
- Monitorear disponibilidad en tiempo real  
- Facilitar estrategias de resiliencia  
- Integrarse con alertamiento 24/7  

Traduce la SP-05 en prácticas obligatorias para desarrollo, QA, producción e infraestructura.

---

# 2. Componentes Fundamentales

La implementación de health checks institucionales requiere **tres mecanismos obligatorios**:

1. **Liveness Probe** — Indica si la aplicación está viva.  
2. **Readiness Probe** — Indica si la aplicación puede recibir tráfico.  
3. **Health Endpoint** — Reporte completo del estado del servicio y dependencias.

---

# 3. Endpoints Obligatorios

Cada servicio debe exponer:

| Endpoint | Descripción |
|----------|-------------|
| `/live` | Liveness: el proceso está funcionando |
| `/ready` | Readiness: el servicio está preparado |
| `/health` o `/healthz` | Estado general del servicio |

Ejemplo JSON estándar:

\```
{
  "status": "UP",
  "version": "1.4.2",
  "timestamp": "2025-01-01T12:00:00Z",
  "checks": {
    "db": "UP",
    "cache": "UP",
    "filesystem": "UP"
  }
}
\```

---

# 4. Decisiones de Estado

## 4.1 Estados válidos
| Estado | Significado |
|--------|-------------|
| `UP` | Operativo |
| `DOWN` | Indisponible |
| `DEGRADED` | Operativo con fallas parciales |

## 4.2 Reglas
- **1 dependencia DOWN → servicio DEGRADED**  
- **Dependencia crítica DOWN → servicio DOWN**  
- **Readiness DOWN → no recibe tráfico**  

---

# 5. Validaciones Técnicas del Health Check

## 5.1 Base de Datos
- Verificar conexión  
- Ejecutar un query mínimo  
- Tiempo máximo recomendado: 100 ms  

## 5.2 Cache (Redis o similar)
- Verificar ping  
- Validar TTL de claves críticas  

## 5.3 Servicios externos
- Llamado liviano (HEAD o GET minimal)  
- Timeout ≤ 1 segundo  

## 5.4 Filesystem
- Validar permisos de escritura en directorios temporales  
- Verificar espacio libre mínimo configurado  

## 5.5 Configuración
- Validar variables críticas cargadas  
- Revisar acceso a secrets vía gestor corporativo  

---

# 6. Ejemplos de Implementación

## 6.1 Spring Boot (Java)
Agregar las dependencias:

\```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
\```

Exponer endpoints:

\```
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.health.show-details=always
\```

## 6.2 Node.js (Express)
\```
app.get('/live', (req, res) => res.json({status: 'UP'}));

app.get('/ready', async (req, res) => {
  const dbOk = await checkDbConnection();
  res.json({status: dbOk ? 'UP' : 'DOWN'});
});
\```

## 6.3 Python (FastAPI)
\```
@app.get("/health")
async def health():
    return {
        "status": "UP",
        "checks": {
            "db": "UP",
            "cache": "UP"
        }
    }
\```

---

# 7. Integración con Sistemas de Orquestación

## 7.1 Kubernetes (recomendado a futuro)

### Liveness
\```
livenessProbe:
  httpGet:
    path: /live
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
\```

### Readiness
\```
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
\```

---

# 8. Integración con la Plataforma de Observabilidad

Cada health check debe:
- Emitirse cada 10 segundos  
- Ser almacenado en la plataforma SIEM/APM  
- Generar alertas automáticas ante estados DOWN o DEGRADED  

---

# 9. Estrategias de Despliegue con Health Checks

## 9.1 Rolling Updates
- Un pod nuevo solo recibe tráfico cuando `/ready = UP`

## 9.2 Blue/Green
- Ambiente "Green" se considera válido solo si health check = UP  
- Si falla, se mantiene "Blue" como producción

## 9.3 Canary
- Se monitorea `/health` de la nueva versión  
- Ante tendencia de degradación → rollback automático

---

# 10. Pruebas en QA

QA debe validar:

### 10.1 Health básico
- `/live`, `/ready`, `/health` responden con JSON válido  
- Tiempo de respuesta < 200 ms  

### 10.2 Health extendido
- Simulación de falla de base de datos  
- Simulación de dependencia externa caída  
- Simulación de filesystem sin espacio  

### 10.3 Alertamiento
- Alertas se disparan en plataforma de monitoreo  
- Logs y métricas reflejan la falla correctamente  

---

# 11. Requisitos Obligatorios en CI/CD

Cada pipeline debe:

- Ejecutar `/ready` antes de marcar despliegue como exitoso  
- Validar `/health` antes y después del deployment  
- Registrar evidencias  
- Garantizar que una aplicación NUNCA reciba tráfico sin readiness UP  

Ejemplo:

\```
curl -f http://app:8080/ready || exit 1
\```

---

# 12. Mejoras Recomendadas (Opcionales)

- Health check firmado digitalmente para integridad  
- Incorporar latencias y métricas en `/health`  
- Dashboard exclusivo de health checks  
- Self-tests avanzados en arranque  

---

# 13. Errores Frecuentes

- ❌ Incorporar lógica de negocio en health checks  
- ❌ Consultas pesadas en health checks  
- ❌ No usar timeouts → ocasiona bloqueos  
- ❌ Permitir que una app sin readiness UP reciba tráfico  
- ❌ Reducir el health check solo a “alive: true”  

---

# FIN DEL MANUAL MT-05
