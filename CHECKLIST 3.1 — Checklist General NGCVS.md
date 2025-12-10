# CHECKLIST GENERAL NGCVS  
**Nivel 3 — Checklist Transversal Obligatorio**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse para poder avanzar de ambiente (DEV → QA → UAT → PRE-PROD → PROD).  
Ningún artefacto puede promoverse sin cumplir **todos** los puntos marcados como “OBLIGATORIO”.

---

# 1. GOBIERNO DEL CICLO DE VIDA (NGCVS)

## 1.1 Cumplimiento documental
- [ ] El proyecto está registrado en el repositorio normativo único  
- [ ] Existe registro de la versión vigente de NGCVS aplicable  
- [ ] El proyecto identifica claramente las Sub-Políticas aplicables (Nivel 1)  
- [ ] El proyecto adoptó los Manuales Técnicos correspondientes (Nivel 2)  
- [ ] Se han completado todos los Checklists Nivel 3 asociados  

## 1.2 Gestión Normativa
- [ ] Existe versión formal del documento funcional  
- [ ] Existe versión formal del documento técnico  
- [ ] Se encuentra actualizado el ciclo documental del proyecto  
- [ ] Auditoría anual marcada como pendiente/completa  
- [ ] No existen deudas normativas abiertas  

---

# 2. ARQUITECTURA Y APLICABILIDAD

## 2.1 Evaluación inicial
- [ ] Presentado el **CETI — Checklist de Elegibilidad Técnica Inicial**  
- [ ] Validadas tecnologías permitidas  
- [ ] Arquitectura revisada y aprobada (Diseño / Infraestructura / Seguridad)  
- [ ] Se definió la matriz de ambientes (DEV/QA/UAT/PRE/PROD)  

## 2.2 Consideraciones estratégicas
- [ ] Se evaluó uso de contenedores Docker (opcional, recomendado)  
- [ ] Se validó compatibilidad con infraestructura institucional  

---

# 3. CONTROL DE VERSIONES (SVN)

## 3.1 Obligatorio
- [ ] Todo el código reside en SVN  
- [ ] Estructura del repositorio cumple estándar institucional  
- [ ] Ramas de desarrollo, QA y producción correctamente identificadas  
- [ ] Commit vinculado a incidente/feature/requerimiento  
- [ ] No existen binarios en SVN  
- [ ] No existen secretos en SVN (validador in-house)  

---

# 4. PIPELINES CI/CD

## 4.1 Construcción y pruebas
- [ ] El artefacto es construido exclusivamente por CI/CD  
- [ ] Se ejecutan pruebas unitarias  
- [ ] Cobertura mínima ≥ 80% aprobada  
- [ ] Existen pruebas automatizadas críticas  
- [ ] Se ejecutan pruebas de integración (si aplica)  

## 4.2 Seguridad
- [ ] SAST completado sin vulnerabilidades críticas  
- [ ] Auditoría de dependencias completada  
- [ ] Escaneo de imágenes (si aplica) sin hallazgos críticos  
- [ ] Validación anti-secreto del validador in-house completada  

## 4.3 Evidencias
- [ ] Artefacto con hash SHA-256  
- [ ] SBOM generado  
- [ ] Logs del pipeline archivados  
- [ ] Reportes de seguridad archivados  
- [ ] Evidencias de pruebas funcionales registradas  
- [ ] Firmas del pipeline (si aplica)  

---

# 5. ARTEFACTOS

- [ ] Versionado con SEMVER  
- [ ] Artefacto almacenado en repositorio corporativo  
- [ ] Hash coincidente con el generado en CI/CD  
- [ ] No existen múltiples versiones del mismo artefacto en producción  
- [ ] CHANGELOG actualizado  

---

# 6. CONFIGURACIONES Y SECRETS MANAGEMENT

- [ ] No existen secretos en código  
- [ ] No existen secretos en archivos .env no cifrados  
- [ ] Gestor de secretos corporativo utilizado  
- [ ] Configuraciones separadas por ambiente  
- [ ] Certificados vigentes  
- [ ] Rotación programada de secretos validada  

---

# 7. OBSERVABILIDAD (LOGS, MÉTRICAS, TRACES, HEALTH)

- [ ] Logging estructurado JSON  
- [ ] Logging con correlation-id  
- [ ] Endpoint `/metrics` operativo  
- [ ] Endpoint `/health` operativo  
- [ ] Endpoint `/ready` y `/live` operativos  
- [ ] Trazabilidad con OpenTelemetry  
- [ ] Dashboards generados  
- [ ] Alertas configuradas  

---

# 8. BASE DE DATOS Y ACCESO A DATOS

- [ ] El modelo de datos fue aprobado por Arquitectura y DBAs  
- [ ] No existen accesos directos al CORE (ver SP-08)  
- [ ] Se usa el esquema de interfaz  
- [ ] GRANTs revisados por DBAs  
- [ ] No existen objetos satélite en el CORE  
- [ ] Auditoría habilitada en la BD  

---

# 9. INFRAESTRUCTURA Y PRODUCCIÓN

- [ ] Diagramas de despliegue actualizados  
- [ ] Monitoreo 24/7 activo  
- [ ] Procedimiento de rollback documentado  
- [ ] Procedimientos de despliegue en CI/CD validados  
- [ ] Revisión de Seguridad previa al paso a producción  
- [ ] Revisión de Producción previa al paso a producción (poder de veto)  

---

# 10. CUMPLIMIENTO PARA PROMOCIÓN ENTRE AMBIENTES

## 10.1 DEV → QA
- [ ] Pruebas unitarias aprobadas  
- [ ] SAST sin críticos  
- [ ] Auditoría de dependencias sin críticos  
- [ ] Artefacto generado por CI/CD  

## 10.2 QA → UAT
- [ ] Pruebas funcionales completadas  
- [ ] QA aprueba formalmente  

## 10.3 UAT → PRE-PROD
- [ ] Usuario o área funcional aprueba  
- [ ] Seguridad técnica revisa  
- [ ] Artefacto no se recompila  

## 10.4 PRE-PROD → PROD
- [ ] Producción aprueba (veto permitido)  
- [ ] Seguridad aprueba (veto permitido)  
- [ ] Procedimiento de rollback validado  
- [ ] Observabilidad operativa comprobada  

---

# 11. VEREDICTO FINAL (obligatorio)
- [ ] **APROBADO PARA PRODUCCIÓN**  
- [ ] **NO APROBADO — BLOQUEADO POR INCUMPLIMIENTOS**  
- Lista de incumplimientos:  
  - _______________________________________________  
  - _______________________________________________  

---

# FIN DEL CHECKLIST GENERAL NGCVS
