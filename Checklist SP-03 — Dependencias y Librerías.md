# CHECKLIST SP-03 — DEPENDENCIAS Y LIBRERÍAS  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD.  
Ningún punto obligatorio puede omitirse.

---

# 1. VERSIONAMIENTO Y CONTROL DE DEPENDENCIAS

## 1.1 Versiones fijas (pinning)
- [ ] Todas las dependencias poseen versión fija  
- [ ] No existen versiones flotantes (`>=`, `^`, `~`, `latest`)  
- [ ] No existen dependencias sin versión especificada  

## 1.2 Lockfiles obligatorios
- [ ] Archivo de lock presente (package-lock.json, Pipfile.lock, go.sum, etc.)  
- [ ] El lockfile está versionado en SVN  
- [ ] El lockfile coincide con las versiones instaladas  
- [ ] El pipeline valida que el lockfile no fue modificado manualmente  

---

# 2. AUDITORÍA DE VULNERABILIDADES

## 2.1 Pruebas automáticas
- [ ] CI/CD ejecuta análisis de dependencias  
- [ ] CI/CD detiene ejecución si hay vulnerabilidades críticas  
- [ ] Vulnerabilidades altas tienen excepción formal o corrección inmediata  
- [ ] En caso de contenedores, el escaneo detecta CVEs en layers base  

## 2.2 Reportes
- [ ] Reporte de auditoría se almacena como evidencia  
- [ ] Reportes SAST y de dependencias están disponibles para auditoría  

---

# 3. DEPENDENCIAS PERMITIDAS / PROHIBIDAS

## 3.1 Repositorios autorizados
- [ ] Dependencias provienen únicamente de repositorios oficiales o corporativos  
- [ ] No existen dependencias instaladas desde URLs externas no autorizadas  
- [ ] No existen dependencias “copiadas manualmente” en el código  

## 3.2 Dependencias prohibidas
- [ ] No existen librerías obsoletas o abandonadas  
- [ ] No existen librerías marcadas como inseguras por Seguridad TICs  
- [ ] No se utilizan frameworks experimentales sin aprobación formal  

---

# 4. INTEGRIDAD Y TRAZABILIDAD DEL COMPONENTE

## 4.1 Hashes
- [ ] Hash del artefacto generado considera dependencias instaladas  
- [ ] Hash se registra como evidencia en CI/CD  

## 4.2 SBOM (Software Bill of Materials)
- [ ] El pipeline genera un SBOM completo  
- [ ] El SBOM está almacenado junto al artefacto  
- [ ] El SBOM lista todas las dependencias directas y transitivas  
- [ ] El SBOM cumple formato CycloneDX o SPDX  

---

# 5. CONTROL DE CALIDAD DE DEPENDENCIAS

- [ ] No existen dependencias duplicadas en el proyecto  
- [ ] No existen dependencias no utilizadas  
- [ ] Las dependencias críticas están justificadas  
- [ ] Las transitive dependencies fueron revisadas  
- [ ] Se revisaron release notes antes de actualizar versiones  

---

# 6. MANTENIMIENTO Y UPDATES

## 6.1 Ciclo de actualización
- [ ] Dependencias críticas actualizadas trimestralmente  
- [ ] Frameworks actualizados semestralmente o según vendor  
- [ ] No existen dependencias con end-of-life (EOL) en PROD  

## 6.2 Pruebas
- [ ] Actualizaciones sometidas a pruebas unitarias  
- [ ] Actualizaciones sometidas a pruebas de integración  
- [ ] QA validó que la actualización no rompe compatibilidad  

---

# 7. VALIDACIONES EN CI/CD

- [ ] CI/CD instala dependencias en modo determinístico (ej: `npm ci`, `pip install --require-hashes`)  
- [ ] Se ejecutan auditorías antes del build  
- [ ] CI/CD falla si:
  - [ ] Faltan hashes  
  - [ ] Falta lockfile  
  - [ ] Hay cambios no autorizados en dependencias  
  - [ ] Hay vulnerabilidades críticas  

---

# 8. CUMPLIMIENTO DE SEGURIDAD

- [ ] No se descargan dependencias durante tiempo de ejecución (runtime fetch)  
- [ ] No existen conexiones salientes a repositorios desconocidos  
- [ ] Dependencias criptográficas cumplen estándares corporativos  
- [ ] No se utiliza código ofuscado o empaquetado sin justificación  

---

# 9. TRAZABILIDAD DOCUMENTAL

- [ ] CHANGELOG incluye actualizaciones de dependencias  
- [ ] Documentación técnica lista dependencias críticas  
- [ ] Cada dependencia tiene justificación técnica en diseño/arquitectura  
- [ ] Auditoría puede reconstruir versiones históricas fácilmente  

---

# 10. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-03 — DEPENDENCIAS Y LIBRERÍAS
