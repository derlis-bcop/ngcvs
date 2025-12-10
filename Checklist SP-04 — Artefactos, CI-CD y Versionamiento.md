# CHECKLIST SP-04 — ARTEFACTOS, CI/CD Y VERSIONAMIENTO  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD.  
Ningún punto obligatorio puede omitirse.

---

# 1. VERSIONAMIENTO FORMAL (SEMVER / ESTÁNDAR CORPORATIVO)

## 1.1 Etiquetado correcto
- [ ] Artefacto versionado siguiendo SEMVER (MAJOR.MINOR.PATCH)  
- [ ] No existen versiones ambiguas (ej. `v1`, `latest`, `snapshot`)  
- [ ] El número de versión coincide con el tag SVN  
- [ ] El pipeline CI/CD asigna la versión automáticamente  

## 1.2 Documentación
- [ ] CHANGELOG actualizado y completo  
- [ ] Versiones documentadas en la solicitud de despliegue  
- [ ] Versiones previas disponibles para rollback inmediato  

---

# 2. INTEGRIDAD DEL ARTEFACTO

## 2.1 Hashes obligatorios
- [ ] Hash SHA-256 generado durante CI/CD  
- [ ] Hash almacenado como artefacto adjunto  
- [ ] Hash validado nuevamente previo al despliegue  

## 2.2 Condiciones
- [ ] Artefacto nunca fue modificado manualmente  
- [ ] Artefacto proviene exclusivamente del pipeline CI/CD  
- [ ] No existen artefactos con nombres duplicados  

---

# 3. CI/CD — PIPELINES OBLIGATORIOS

## 3.1 Build
- [ ] El build se ejecuta únicamente via pipeline  
- [ ] No existen builds locales subidos a SVN  
- [ ] El pipeline usa herramientas homologadas (maven, npm, etc.)  

## 3.2 Tests
- [ ] Pruebas unitarias ejecutadas automáticamente  
- [ ] Cobertura ≥ 80%  
- [ ] Pruebas de integración ejecutadas (si aplica)  
- [ ] Evidencias almacenadas en el pipeline  

## 3.3 Seguridad
- [ ] SAST ejecutado  
- [ ] Auditoría de dependencias ejecutada  
- [ ] Escaneo Docker ejecutado (cuando aplica)  
- [ ] No hay vulnerabilidades críticas sin excepción formal  

---

# 4. PROMOCIÓN ENTRE AMBIENTES

## 4.1 Flujo obligatorio
- [ ] DEV → QA  
- [ ] QA → UAT  
- [ ] UAT → PRE-PROD  
- [ ] PRE-PROD → PROD  

## 4.2 Controles
- [ ] Promoción realizada solo desde CI/CD  
- [ ] No se recompila entre ambientes  
- [ ] No se modifican configuraciones manualmente  
- [ ] No se copian artefactos directamente a servidores  
- [ ] Aprobaciones registradas y trazadas  

---

# 5. REPOSITORIO DE ARTEFACTOS

- [ ] Artefacto cargado en repositorio corporativo  
- [ ] Repositorio mantiene historial completo  
- [ ] No existen artefactos huérfanos o sin versionar  
- [ ] SBOM almacenado junto al artefacto  

---

# 6. EVIDENCIAS DEL PIPELINE

- [ ] Reportes SAST adjuntos  
- [ ] Reportes de dependencias adjuntos  
- [ ] Logs completos del pipeline archivados  
- [ ] Evidencia de pruebas unitarias  
- [ ] Evidencia de pruebas funcionales  
- [ ] Artefacto + hash + SBOM almacenados juntos  

---

# 7. CONTROL DE CONFIGURACIONES

- [ ] Variables de entorno documentadas  
- [ ] No existen secretos hardcodeados  
- [ ] Configuración se pasa fuera del artefacto (12-factor)  
- [ ] Configuraciones por ambiente correctamente definidas  

---

# 8. ROLLBACK FORMAL

- [ ] Existe procedimiento de rollback documentado  
- [ ] Artefactos previos disponibles  
- [ ] Hash del artefacto previo verificado  
- [ ] QA validó el rollback  
- [ ] Producción confirmó la viabilidad del rollback  

---

# 9. AUDITORÍA Y TRAZABILIDAD

- [ ] Todo despliegue queda registrado  
- [ ] Logs de CI/CD accesibles para auditoría  
- [ ] Aprobaciones firmadas por responsables  
- [ ] Vinculación artefacto–commit–ticket de requerimiento  
- [ ] Evidencias guardadas ≥ 12 meses  

---

# 10. ERRORES A DETECTAR (IMPOSIBILITAN PROMOVER)

- [ ] Build manual  
- [ ] Tag sin correspondencia a commit válido  
- [ ] Hash inexistente o incorrecto  
- [ ] Múltiples versiones del artefacto para un mismo despliegue  
- [ ] Falta de SAST o dependencias sin analizar  
- [ ] Artefactos modificados tras build  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-04 — ARTEFACTOS, CI/CD Y VERSIONAMIENTO
