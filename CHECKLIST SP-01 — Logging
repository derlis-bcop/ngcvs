# CHECKLIST SP-01 — LOGGING  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse antes de promover cualquier artefacto a QA, UAT, PRE-PROD o PROD.  
La ausencia de cualquier punto obligatorio impide avanzar entre ambientes.

---

# 1. FORMATO Y ESTRUCTURA DEL LOG

## 1.1 Formato Estructurado JSON
- [ ] Todos los logs se generan en formato JSON válido  
- [ ] El formato JSON incluye claves obligatorias:
  - timestamp  
  - level  
  - service  
  - environment  
  - correlation_id  
  - operation  
  - message  
- [ ] Los logs incluyen `duration_ms` cuando corresponde  
- [ ] Los logs de errores incluyen `exception` con stacktrace  

## 1.2 Validación sintáctica
- [ ] Pruebas automáticas validan JSON bien formado  
- [ ] No existen logs en texto plano  

---

# 2. CORRELATION-ID

- [ ] Toda solicitud entrante genera o reutiliza un correlation-id  
- [ ] El correlation-id se propaga a servicios downstream  
- [ ] El correlation-id aparece en:
  - Logs  
  - Métricas  
  - Trazas  
  - Alertas  

---

# 3. NIVELES DE LOGGING

- [ ] Nivel DEBUG está desactivado en UAT/PRE/PROD  
- [ ] INFO, WARN, ERROR, FATAL utilizados correctamente  
- [ ] No hay abuso de logging (log spam) en loops o procesos masivos  
- [ ] Los mensajes cumplen una narrativa clara y breve  

---

# 4. SEGURIDAD DEL LOGGING

- [ ] No se registran datos sensibles (passwords, tokens, API keys, datos personales)  
- [ ] El validador in-house detecta e impide secretos en logs  
- [ ] Logs de error NO incluyen objetos completos ni blobs  
- [ ] No se imprimen datos confidenciales en DEBUG  

---

# 5. ENVÍO A PLATAFORMA CENTRALIZADA

- [ ] El servicio envía logs automáticamente a la plataforma de observabilidad  
- [ ] Se utilizan agentes certificados (Fluentd, Filebeat, OpenTelemetry, etc.)  
- [ ] Logs viajan por TLS  
- [ ] No existen envíos directos a endpoints externos no autorizados  

---

# 6. CONSISTENCIA POR AMBIENTE

- [ ] El campo `environment` está correctamente configurado  
- [ ] No existen logs que indiquen manualmente el ambiente (“AMB: DEV”, etc.)  
- [ ] Logs distinguen claramente eventos entre DEV, QA, UAT, PRE-PROD y PROD  

---

# 7. DASHBOARDS Y CONSULTAS

- [ ] Dashboards basados en logs fueron creados  
- [ ] Existe al menos:
  - Dashboard de errores  
  - Dashboard de performance  
  - Dashboard de correlación por operación  
- [ ] Consultas predefinidas para análisis de incidentes están disponibles  

---

# 8. AUDITORÍA

- [ ] Retención mínima de logs cumple la política institucional (≥ 12 meses para PROD)  
- [ ] Auditoría puede reconstruir eventos completos por correlation-id  
- [ ] Se verificó integridad de logs: no modificables, no borrables  
- [ ] No existen huecos en los registros (periodos sin logging)  

---

# 9. PIPELINE CI/CD — VALIDACIONES AUTOMÁTICAS

- [ ] Pipeline verifica JSON válido  
- [ ] Pipeline detecta secretos expuestos  
- [ ] Pipeline valida presence de correlation-id  
- [ ] Pipeline falla si se intenta loggear datos sensibles  
- [ ] Evidencias del pipeline incluyen logs de ejecución  

---

# 10. OPERACIÓN Y PRODUCCIÓN

- [ ] Alertas basadas en logs están configuradas  
- [ ] Alertas críticas notifican a 24/7 automáticamente  
- [ ] Los logs permiten detectar degradación previo a incidentes  
- [ ] Logs de despliegue están registrados en CI/CD  

---

# 11. VEREDICTO FINAL
- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-01 — LOGGING
