# CHECKLIST SP-02 — MONITOREO Y OBSERVABILIDAD  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse antes de promover cualquier artefacto a QA, UAT, PRE-PROD o PROD.  
La ausencia de cualquier punto obligatorio impide avanzar entre ambientes.

---

# 1. ENDPOINTS Y EXPOSICIÓN DE MÉTRICAS

## 1.1 Endpoints obligatorios
- [ ] `/metrics` operativo y accesible  
- [ ] `/live` operativo  
- [ ] `/ready` operativo  
- [ ] `/health` operativo  

## 1.2 Validaciones técnicas
- [ ] `/metrics` devuelve formato Prometheus válido  
- [ ] `/health` incluye estado general y dependencias  
- [ ] `/ready` responde según disponibilidad real  
- [ ] `/live` responde independientemente de dependencias externas  

---

# 2. MÉTRICAS TÉCNICAS OBLIGATORIAS

## 2.1 Recolección estándar
- [ ] CPU del proceso expuesta  
- [ ] Uso de memoria expuesto  
- [ ] Conexiones activas expuestas  
- [ ] Threads activos expuestos  
- [ ] Latencia de peticiones expuesta (p50, p90, p99)  
- [ ] Throughput (requests_total) expuesto  
- [ ] Errores por tipo expuestos  
- [ ] Duración por endpoint registrada  

## 2.2 Métricas de dependencias
- [ ] Métricas de base de datos expuestas  
- [ ] Métricas de cache (Redis u otro) expuestas  
- [ ] Métricas http_client para servicios externos  

---

# 3. TRACING (TRAZAS)

## 3.1 Instrumentación
- [ ] OpenTelemetry habilitado  
- [ ] Spans generados por operación  
- [ ] Propagación correcta de `traceparent`  
- [ ] Logs, métricas y trazas vinculados por correlation-id  

## 3.2 Validación en ambiente
- [ ] Trazas visibles en la plataforma de observabilidad  
- [ ] Se visualizan spans de servicios internos  
- [ ] Se visualizan spans de llamados a terceros  
- [ ] Trazas reflejan fallos y timeouts  

---

# 4. LOGS + MÉTRICAS + TRAZAS

- [ ] Los logs incluyen correlation-id  
- [ ] Las trazas incluyen correlation-id  
- [ ] Las métricas están etiquetadas con correlation-id (cuando aplica)  
- [ ] Se puede reconstruir una operación completa desde un tablero  

---

# 5. ALERTAS Y MONITOREO

## 5.1 Reglas de alertas
- [ ] Alerta de servicio DOWN configurada  
- [ ] Alerta de readiness caída configurada  
- [ ] Alerta de latencia > p99 configurada  
- [ ] Alerta de error rate > 5% configurada  
- [ ] Alerta por falta de métricas (“silencio”) configurada  

## 5.2 Integración con 24/7
- [ ] Grupo 24/7 recibe notificaciones  
- [ ] Alertas críticas generan llamadas o mensajes inmediatos  
- [ ] No existen alertas silenciadas indebidamente  

---

# 6. DASHBOARDS OBLIGATORIOS

## 6.1 Dashboard de Performance
- [ ] Latencia por endpoint  
- [ ] Throughput por minuto  
- [ ] Histogramas y percentiles  

## 6.2 Dashboard de Recursos
- [ ] CPU  
- [ ] Memoria  
- [ ] IO / conexiones  

## 6.3 Dashboard de Salud
- [ ] Estados de health checks  
- [ ] Disponibilidad histórica  
- [ ] Alertas activas y recientes  

---

# 7. INTEGRACIÓN CON CI/CD

- [ ] El pipeline valida `/ready` antes de finalizar despliegue  
- [ ] El pipeline genera evidencia de monitoreo previo y posterior  
- [ ] CI/CD falla si `/health` no está operativo  
- [ ] CI/CD registra pruebas sintéticas (si aplican)  

---

# 8. PRUEBAS DE QA

## 8.1 Validaciones funcionales
- [ ] Health checks probados en condiciones normales  
- [ ] Health checks probados con servicios externos caídos  
- [ ] Validación de latencia en endpoints críticos  
- [ ] Simulación de degradación del cache  
- [ ] Simulación de error en base de datos  

## 8.2 Integración
- [ ] Métricas detectadas por el agente institucional  
- [ ] Logs, métricas y trazas alineados correctamente  

---

# 9. ESTABILIDAD OPERATIVA

- [ ] No existen huecos de datos en métricas  
- [ ] El servicio se mantiene observable durante despliegues  
- [ ] Readiness evita tráfico hacia pods o instancias no listas  
- [ ] Alertas tempranas de degradación están funcionando  

---

# 10. AUDITORÍA DE OBSERVABILIDAD

- [ ] Retención de métricas ≥ a la política institucional  
- [ ] Retención de trazas según criticidad del sistema  
- [ ] Evidencia de auditoría disponible  
- [ ] No existen endpoints ocultos o no documentados  
- [ ] Documentación operacional actualizada  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-02 — MONITOREO Y OBSERVABILIDAD
