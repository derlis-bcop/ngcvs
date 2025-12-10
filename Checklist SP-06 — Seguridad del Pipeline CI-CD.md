# CHECKLIST SP-06 — SEGURIDAD DEL PIPELINE CI/CD  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist garantiza que el pipeline CI/CD cumple estándares de seguridad, integridad, auditoría y control establecidos en NGCVS, SP-06 y MT-06.  
Debe completarse antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD.

---

# 1. INTEGRIDAD DEL PIPELINE

## 1.1 Configuración versionada
- [ ] El pipeline CI/CD está versionado en SVN  
- [ ] No existen configuraciones de pipeline fuera del repositorio  
- [ ] Cambios en pipeline tienen aprobación formal  
- [ ] No existen pipelines paralelos o alternativos no autorizados  

## 1.2 Inmutabilidad
- [ ] El pipeline NO permite ejecución manual de pasos alterados  
- [ ] No existen scripts modificados en servidores  
- [ ] Build siempre es reproducible  

---

# 2. SEGURIDAD DE RUNNERS / AGENTS

## 2.1 Hardening
- [ ] Runners actualizados y parchados  
- [ ] Sin software no autorizado instalado  
- [ ] Acceso restringido por firewall  

## 2.2 Principio de mínimo privilegio
- [ ] Runner no tiene permisos de administración de servidores productivos  
- [ ] Runner no tiene acceso directo a base de datos  
- [ ] Runner no almacena secretos en disco  

## 2.3 Aislamiento por ambiente
- [ ] Runners de PROD son exclusivos  
- [ ] Runners de DEV/QA no pueden acceder a recursos productivos  

---

# 3. SECRETS MANAGEMENT

- [ ] No existen secretos en variables de pipeline en texto plano  
- [ ] No existen secretos en repositorios  
- [ ] Secrets almacenados únicamente en gestor corporativo  
- [ ] El pipeline obtiene secretos dinámicamente (no hardcode)  
- [ ] Rotación de secretos documentada y ejecutada  
- [ ] No existe exposición de secretos en logs del pipeline  

---

# 4. ANÁLISIS DE SEGURIDAD AUTOMÁTICOS

## 4.1 SAST
- [ ] Análisis SAST se ejecuta en cada pipeline  
- [ ] No hay vulnerabilidades críticas  
- [ ] Vulnerabilidades altas tienen excepción o corrección inmediata  

## 4.2 Auditoría de dependencias
- [ ] Dependencias analizadas en cada build  
- [ ] No existen CVEs críticos  
- [ ] Reportes guardados como evidencia  

## 4.3 Escaneo de imágenes (si aplica)
- [ ] Trivy u otra herramienta homologada ejecutada  
- [ ] Vulnerabilidades críticas bloquean pipeline  

## 4.4 Validador in-house
- [ ] Detección de secretos  
- [ ] Validación de estándares de código  
- [ ] Validación de formato y estructura  

---

# 5. INTEGRIDAD DEL ARTEFACTO

- [ ] Artefacto generado únicamente por pipeline  
- [ ] Hash SHA-256 generado y verificado  
- [ ] SBOM generado  
- [ ] No existen modificaciones post-build  
- [ ] Artefacto publicado en repositorio corporativo  

---

# 6. TRAZABILIDAD Y EVIDENCIAS

- [ ] Artefacto vinculado a commit SVN  
- [ ] Pipeline registra:
  - [ ] Número de versión  
  - [ ] Autor del commit  
  - [ ] Fecha y hora del build  
  - [ ] Cambios desplegados  
- [ ] Evidencias de pruebas unitarias e integración  
- [ ] Evidencia de análisis SAST  
- [ ] Evidencia de auditoría de dependencias  
- [ ] Evidencia de pruebas funcionales en UAT  

---

# 7. CONTROL DE PROMOCIÓN DE AMBIENTES

- [ ] Artefacto NO se recompila  
- [ ] Artefacto NO se modifica  
- [ ] Promoción solo se realiza via pipeline  
- [ ] Requiere aprobaciones electrónicas trazadas:
  - [ ] QA  
  - [ ] Seguridad  
  - [ ] Producción  

- [ ] Deploy a producción requiere doble aprobación (Seguridad + Producción)  
- [ ] Producción mantiene poder de veto  

---

# 8. SEGURIDAD EN DESPLIEGUES

## 8.1 Restricciones
- [ ] No existen accesos manuales no autorizados  
- [ ] No se permite copiar archivos a servidores sin pipeline  
- [ ] No se ejecutan comandos manuales en servidores de PROD  

## 8.2 Validación operativa
- [ ] `/ready` validado post-deploy  
- [ ] `/health` validado en smoke test  
- [ ] Logs y métricas visibles en monitoreo 24/7  

---

# 9. AUDITORÍA DEL PIPELINE

- [ ] Todos los logs del pipeline son inmutables  
- [ ] Logs están almacenados ≥ 12 meses  
- [ ] Auditoría puede reconstruir:
  - [ ] Qué se desplegó  
  - [ ] Quién aprobó  
  - [ ] Por qué  
  - [ ] Con qué evidencias  

---

# 10. ERROR CRÍTICO QUE IMPIDE PROMOVER

- [ ] Falta ejecución de SAST  
- [ ] Falta auditoría de dependencias  
- [ ] Falta hash o mismatch detectado  
- [ ] Artefacto modificado fuera del pipeline  
- [ ] Hardcode de secretos detectado  
- [ ] Runners con permisos excesivos  
- [ ] No existen evidencias del pipeline  
- [ ] Falta aprobación de Producción o Seguridad  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-06 — SEGURIDAD DEL PIPELINE CI/CD
