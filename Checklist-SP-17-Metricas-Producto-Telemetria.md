# CHECKLIST SP-17 — MÉTRICAS DE PRODUCTO Y TELEMETRÍA  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-17 y MT-17

Este checklist es obligatorio para validar cualquier sistema antes de su promoción a QA, UAT, PRE-PROD o PROD.  
El incumplimiento de cualquier punto obligatorio impide avanzar de ambiente.

---

# 1. EXISTENCIA Y ACCESIBILIDAD DEL ENDPOINT DE MÉTRICAS

- [ ] Existe endpoint `/metrics` o equivalente institucional  
- [ ] El endpoint responde correctamente en todos los ambientes  
- [ ] El formato es compatible con la plataforma de observabilidad  
- [ ] El endpoint no requiere autenticación para métricas técnicas básicas  
- [ ] El endpoint requiere autenticación cuando se exponen métricas internas de producto  
- [ ] La respuesta no degrada la performance (respuesta < 50 ms)  
- [ ] No altera el estado del sistema al ser consultado  

---

# 2. PRESENCIA DE MÉTRICAS TÉCNICAS OBLIGATORIAS

## 2.1 Latencia
- [ ] Latencia promedio expuesta  
- [ ] p95 expuesto  
- [ ] p99 expuesto  

## 2.2 Disponibilidad
- [ ] Métrica de disponibilidad presente  
- [ ] Estado del servicio expuesto correctamente  

## 2.3 Throughput
- [ ] Solicitudes por segundo/minuto registradas  
- [ ] Total acumulado de operaciones  

## 2.4 Errores
- [ ] Errores 4xx contabilizados  
- [ ] Errores 5xx contabilizados  
- [ ] Ratio de error generado  

## 2.5 Dependencias externas
- [ ] Métricas de BD  
- [ ] Métricas de cache  
- [ ] Métricas de servicios externos  
- [ ] Métricas de colas o mensajería  

## 2.6 Recursos (si aplica)
- [ ] CPU  
- [ ] Memoria  
- [ ] Conexiones activas  

---

# 3. MÉTRICAS DE PRODUCTO

- [ ] Existen métricas del dominio funcional  
- [ ] Volumen de transacciones expuesto  
- [ ] Métricas clave definidas por el Product Owner  
- [ ] Flujo del usuario medido apropiadamente  
- [ ] No se exponen valores sensibles o identificables  
- [ ] Existen labels adecuados (operation, status, tipo_transaccion, etc.)  

---

# 4. TELEMETRÍA OPERACIONAL

### 4.1 Eventos críticos
- [ ] Timeouts registrados  
- [ ] Reintentos registrados  
- [ ] Degradaciones registradas  
- [ ] Fallos internos registrados  

### 4.2 Procesos batch y jobs
- [ ] Inicio del job registrado  
- [ ] Fin del job registrado  
- [ ] Estado final (success/error) expuesto  
- [ ] Volumen de procesamiento expuesto  
- [ ] Tiempo total de ejecución expuesto  
- [ ] Reintentos expuestos  

### 4.3 Identificadores de trazabilidad
- [ ] Cada request genera `trace_id`  
- [ ] `trace_id` propagado a logs, métricas y telemetría  
- [ ] `correlation_id` presente para flujos distribuidos  

---

# 5. VALIDACIONES DE DATOS SENSIBLES (OBLIGATORIO)

- [ ] No existen nombres, apellidos o identificadores personales  
- [ ] No existen números de documento  
- [ ] No existen datos financieros  
- [ ] No existen contenidos de payloads de usuario  
- [ ] No existen hashes, tokens o claves  
- [ ] No se filtran mensajes de error internos  
- [ ] No se exponen datos de autenticación  

**Si se detecta cualquier dato personal → RECHAZADO.**

---

# 6. NORMAS DE FORMATO Y NOMENCLATURA

- [ ] Métrica sigue convención institucional `<tics>_<dominio>_<componente>_<nombre>`  
- [ ] Labels siguen convención institucional (`app`, `env`, `version`, `status`, `operation`)  
- [ ] Todas las métricas tienen descripción clara  
- [ ] No existen cambios de nombre sin versionado  
- [ ] No existen métricas duplicadas  

---

# 7. CI/CD — VALIDACIONES AUTOMÁTICAS

- [ ] Pipeline verifica existencia de `/metrics`  
- [ ] Pipeline valida formato  
- [ ] Pipeline detecta datos sensibles  
- [ ] Pipeline ejecuta pruebas de integración generando telemetría  
- [ ] Pipeline registra evidencias  
- [ ] Cualquier falla → pipeline se detiene  

---

# 8. VALIDACIÓN EN QA Y UAT

- [ ] Métricas reflejan el comportamiento real de las pruebas  
- [ ] Métricas visibles en dashboards institucionales  
- [ ] Telemetría generada en escenarios funcionales  
- [ ] Latencias esperadas validadas  
- [ ] Eventos de negocio registrados correctamente  

---

# 9. VALIDACIÓN EN PRE-PROD Y PROD

- [ ] Ingesta exitosa de métricas en monitoreo centralizado  
- [ ] Dashboards completos y actualizados  
- [ ] Alertas configuradas según KPIs críticos  
- [ ] Series temporales disponibles para auditoría  
- [ ] Trazabilidad completa de incidentes posible  

---

# 10. AUDITORÍA Y RETENCIÓN

- [ ] Métricas almacenadas ≥ 12 meses  
- [ ] Trazabilidad disponible para cada versión del sistema  
- [ ] Métricas exportables para auditoría  
- [ ] Catalogación de métricas entregada por el equipo  
- [ ] Evaluación anual por Arquitectura, QA y Seguridad TICs  

---

# 11. ERRORES QUE IMPIDEN PROMOVER ENTRE AMBIENTES

- [ ] Falta endpoint `/metrics`  
- [ ] Formato no estándar  
- [ ] Datos sensibles presentes  
- [ ] Ausencia de métricas críticas (latencia, errores, disponibilidad)  
- [ ] Ausencia de trazas (`trace_id`, `correlation_id`)  
- [ ] Ausencia de métricas del negocio  
- [ ] Pipeline no valida métricas  
- [ ] Dashboards no disponibles  
- [ ] Métricas inconsistentes con comportamiento real  
- [ ] Falta de ingestión en la plataforma central  

---

# 12. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Observaciones:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-17 — MÉTRICAS DE PRODUCTO Y TELEMETRÍA
