# MT-17 — MANUAL TÉCNICO DE MÉTRICAS DE PRODUCTO Y TELEMETRÍA  
**Manual Técnico — Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-17 — Métricas de Producto y Telemetría

---

# 1. Propósito Operativo
Este Manual Técnico define los estándares, lineamientos y configuraciones necesarias para la correcta implementación, recolección, procesamiento, publicación, validación y almacenamiento de métricas de producto y telemetría en todos los sistemas desarrollados o administrados por la Gerencia de TICs.

Este documento operacionaliza SP-17, asegurando:

- Observabilidad completa del sistema  
- Trazabilidad de eventos, flujos e incidentes  
- Datos cuantitativos para la toma de decisiones  
- Prevención de degradación silenciosa  
- Cumplimiento de requisitos de auditoría  
- Mecanismos estandarizados para métricas y telemetría  

---

# 2. Tipos de Métricas Requeridas

## 2.1 Métricas Técnicas Obligatorias
Todo sistema deberá instrumentar, como mínimo:

1. **Latencia**  
   - promedio, p50, p95, p99  
2. **Disponibilidad del servicio**  
3. **Throughput**  
   - solicitudes por segundo / minuto  
4. **Errores por categoría**  
   - 4xx (errores del cliente)  
   - 5xx (errores del servidor)  
5. **Estado de dependencias**  
   - base de datos  
   - cache  
   - servicios externos  
6. **Consumo de recursos** (cuando aplique)  
   - CPU  
   - memoria  
   - disco  
   - conexiones abiertas  
7. **Colas y Jobs**  
   - tiempo en cola  
   - tiempo de procesamiento  

---

## 2.2 Métricas de Producto
Las métricas de producto deben ser definidas junto con:

- Product Owner  
- Arquitectura  
- Seguridad TICs (para evitar datos sensibles)  

Ejemplos:

- Cantidad de transacciones exitosas/fallidas  
- Flujo de usuarios por paso  
- Uso de funcionalidades clave  
- Aprobaciones/rechazos  
- Tiempos de ejecución de operaciones críticas  
- Eventos relevantes del negocio  

---

## 2.3 Métricas de Telemetría Operacional
Las aplicaciones deben generar telemetría útil para:

- diagnóstico  
- auditoría  
- reconstrucción de incidentes  
- análisis de rendimiento  
- patrones de uso  

Telemetría obligatoria:

- Identificador único de transacción  
- Identificador de correlación distribuida  
- Trazas por request  
- Eventos críticos (timeouts, degradaciones, reintentos, circuit breakers)

---

# 3. Estándares Técnicos de Instrumentación

## 3.1 Convenciones de Nombres (Naming)
Toda métrica debe seguir el formato:

```
<tics><dominio><componente>_<nombre>
```

Ejemplos:

- `tics_api_login_latency_ms`  
- `tics_core_transacciones_total`  

---

## 3.2 Etiquetas (Labels)
Deben utilizarse etiquetas homogéneas:

- `app`  
- `version`  
- `env`  
- `status`  
- `operation`  
- `endpoint`  

Ejemplo recomendado en pseudo-métrica:

```
tics_api_pagos_latency_ms{env="prod",operation="pago_tarjeta"} 120
```

---

## 3.3 Métricas sin Datos Sensibles
Queda prohibido incluir:

- Nombres completos  
- Documentos personales  
- Datos financieros  
- Tokens, claves, hashes  
- Datos de autenticación  

---

# 4. Endpoint de Métricas

## 4.1 Requisitos obligatorios
Cada aplicación debe exponer un endpoint:

- `/metrics`  
- o uno definido por la plataforma institucional  

Debe cumplir:

- Ser accesible desde la plataforma de observabilidad  
- No generar carga significativa  
- No exponer información sensible  
- No alterar el estado del sistema  

---

## 4.2 Formato de Métricas
El formato estándar deberá ser compatible con:

- Prometheus (texto plano)  
- O cualquier formato homologado por Arquitectura/Infraestructura  

Ejemplo de salida simplificada (escapado):

\```
# HELP tics_api_login_latency_ms Latencia del endpoint de login
# TYPE tics_api_login_latency_ms histogram
tics_api_login_latency_ms_bucket{le="100"} 23
tics_api_login_latency_ms_bucket{le="250"} 50
tics_api_login_latency_ms_sum 12345
tics_api_login_latency_ms_count 300
\```

---

# 5. Trazabilidad y Correlación

## 5.1 Identificadores obligatorios
Cada request debe generar:

- `trace_id`  
- `span_id`  
- `correlation_id`  

Estos deben propagarse a:

- logs  
- métricas  
- telemetría  
- eventos internos  

---

## 5.2 Integración con Logging y Observabilidad
Métricas deben complementarse con:

- logging estructurado (SP-01)  
- monitoreo (SP-02)  
- health checks (SP-05)  

La combinación debe permitir reconstruir:

- flujos de usuario  
- tiempos críticos  
- incidentes operativos  
- degradaciones funcionales  

---

# 6. Telemetría en Procesos Batch o Jobs

Todo proceso batch debe generar:

1. Inicio del job  
2. Fin del job  
3. Estado final (success/error)  
4. Cantidad de registros procesados  
5. Tiempo total de ejecución  
6. Cantidad de reintentos  
7. Errores detallados sin información sensible  

---

# 7. Telemetría en Eventos de Negocio

Los eventos relevantes deben generar métricas y telemetría suficientes para auditoría.  
Ejemplos:

- creación de usuario  
- pago procesado  
- alta de contrato  
- aprobación / rechazo  

Todos deben ser **anonimizados**, bajo estrictas reglas de seguridad.

---

# 8. Validación Automatizada en CI/CD

Los pipelines deben validar que:

- [ ] El endpoint `/metrics` existe  
- [ ] El formato es válido  
- [ ] No hay datos sensibles  
- [ ] Métricas responden rápido (<50 ms)  
- [ ] Métricas se generan en ambientes controlados  
- [ ] Trazas se propagan correctamente  

Si falla alguna → **pipeline bloqueado**.

---

# 9. Prácticas Obligatorias

- Instrumentación desde fase inicial del proyecto  
- Métricas y telemetría versionadas con el código  
- No alterar métricas en tiempo de ejecución sin pasar por CI/CD  
- Revisión periódica por Arquitectura y Seguridad TICs  
- Documentación clara del catálogo de métricas  

---

# 10. Anti-Patrones (Prohibido)

- Enviar métricas excesivas que afecten performance  
- Generar métricas por cada iteración de loops intensivos  
- Exponer errores internos en métricas  
- Tener múltiples endpoints `/metrics` no coordinados  
- Modificar nombres de métricas sin versionado  
- Incluir payloads o JSON completos como métrica  

---

# 11. Almacenamiento y Retención

- Métricas deben almacenarse en plataforma institucional  
- Retención mínima de **12 meses** para métricas críticas  
- Métricas deben ser exportables para auditoría  
- Históricos deben ser consultables por servicio, día y versión  

---

# 12. Visualización (Dashboards)

Cada equipo debe mantener dashboards con:

- Métricas técnicas (latencia, errores, disponibilidad)  
- Métricas de producto (transacciones, eventos clave)  
- Métricas de telemetría operacional (procesos internos)  
- Estado de servicios dependientes  

Los dashboards forman parte de los artefactos del proyecto.

---

# 13. Ejemplos de Instrumentación  
(Se presentan en forma conceptual para cumplir con requisitos de escape)

### Ejemplo de contador
\```
metricas.incrementar("tics_api_pagos_total", labels={"status":"ok"})
\```

### Ejemplo de histograma
\```
metricas.registrar_tiempo("tics_api_login_latency_ms", elapsed_ms)
\```

### Ejemplo de trazabilidad
\```
trace_id = generar_trace_id()
logger.info("Operacion exitosa", trace_id=trace_id)
\```

---

# 14. Errores que Bloquean Despliegues

- Falta de endpoint `/metrics`  
- Métricas mal formadas  
- Datos sensibles en cualquier métrica  
- Falta de telemetría crítica  
- Latencia excesiva en generación de métricas  
- Ausencia de trazas distribuidas  
- No cumplimiento de estándares de nombre  
- Falta de dashboard mínimo de operación  

---

# 15. Relación con Otros Documentos

- NGCVS  
- SP-01 Logging  
- SP-02 Monitoreo  
- SP-05 Health Checks  
- SP-14 Pruebas Automatizadas  
- SP-17 Políticas de Métricas  
- Checklists Nivel 3 asociados  

---

# FIN DEL MANUAL TÉCNICO MT-17
