# CHECKLIST SP-14 — PRUEBAS AUTOMATIZADAS  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-14 y MT-14

Este checklist es obligatorio para auditar cualquier artefacto antes de su promoción entre ambientes (DEV → QA → UAT → PRE-PROD → PROD).  
El incumplimiento de cualquier punto obligatorio impide avanzar de ambiente.

---

# 1. ESTRUCTURA Y EXISTENCIA DE PRUEBAS

## 1.1 Presencia de pruebas automatizadas
- [ ] Existen pruebas unitarias  
- [ ] Existen pruebas de integración  
- [ ] Existen pruebas de regresión automatizada  
- [ ] Existen pruebas funcionales automatizadas (si aplica)  
- [ ] Existen pruebas de seguridad automatizadas (SAST, dependencias)  

## 1.2 Estructura estandarizada del proyecto
- [ ] Carpetas de pruebas correctamente organizadas  
- [ ] Pruebas versionadas en SVN  
- [ ] Pruebas separadas del código de aplicación  
- [ ] Configuración clara de test runner disponible  
- [ ] Archivo de configuración de cobertura presente  

---

# 2. COBERTURA DE CÓDIGO (OBLIGATORIA)

- [ ] Cobertura ≥ **80%**  
- [ ] Cobertura generada automáticamente en CI/CD  
- [ ] Reportes incluyen:
  - [ ] Porcentaje total  
  - [ ] Porcentaje por módulo  
  - [ ] Líneas no cubiertas  
- [ ] Reportes accesibles como artefactos del pipeline  
- [ ] Evidencias almacenadas para auditoría  

**Si cobertura < 80% → RECHAZADO.**

---

# 3. CALIDAD DE PRUEBAS UNITARIAS

- [ ] Las pruebas unitarias son determinísticas  
- [ ] No dependen de BD real  
- [ ] No dependen de servicios externos reales  
- [ ] Utilizan mocks/stubs correctamente  
- [ ] Pruebas ejecutan rápido (<100 ms por prueba cuando aplica)  
- [ ] No utilizan `sleep`, tiempos variables o aleatoriedad  

---

# 4. PRUEBAS DE INTEGRACIÓN

- [ ] Existen pruebas de integración para módulos clave  
- [ ] Las pruebas están aisladas del ambiente productivo  
- [ ] Datos de prueba controlados  
- [ ] Logs no contienen información sensible  
- [ ] Pruebas ejecutan correctamente en CI/CD  
- [ ] Errores capturados sin exponer información sensible  

---

# 5. PRUEBAS FUNCIONALES (SI APLICA)

- [ ] Validan flujos completos de negocio  
- [ ] No dependen de datos mutables  
- [ ] Escenarios cubren casos críticos  
- [ ] Herramienta funcional homologada (Selenium, Playwright, Cypress, etc.)  
- [ ] Evidencias de ejecución almacenadas  

---

# 6. PRUEBAS DE REGRESIÓN AUTOMATIZADA

- [ ] Suite de regresión diseñada para detectar impactos colaterales  
- [ ] Incluye pruebas para:
  - [ ] Funcionalidades críticas  
  - [ ] Endpoints principales  
  - [ ] Procesos batch  
  - [ ] Integraciones clave  
- [ ] Se ejecuta automáticamente en QA y UAT  
- [ ] Evidencias disponibles  

---

# 7. PRUEBAS DE PERFORMANCE (SI APLICA)

- [ ] Se utiliza JMeter, k6 o herramienta homologada  
- [ ] Están definidas cargas máximas y límites aceptables  
- [ ] Resultados archivados en CI/CD  
- [ ] No se detectan
