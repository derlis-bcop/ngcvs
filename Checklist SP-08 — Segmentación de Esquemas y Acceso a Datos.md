# CHECKLIST SP-08 — SEGMENTACIÓN DE ESQUEMAS Y ACCESO A DATOS  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist valida el cumplimiento estricto de la Sub-Política SP-08 y el Manual Técnico MT-08.  
Debe completarse antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD.

---

# 1. ESQUEMAS DEFINIDOS Y ESTRUCTURA ADECUADA

## 1.1 Existencia de esquemas institucionales
- [ ] Esquema CORE creado y documentado  
- [ ] Esquema INTERFAZ creado y documentado  
- [ ] Esquema SATÉLITE creado por cada aplicación  
- [ ] Estructura respeta la convención:  
  - `CORE_<sistema>`  
  - `INT_<sistema>`  
  - `SAT_<aplicación>`  

## 1.2 Validación documental
- [ ] Arquitectura aprobó la estructura  
- [ ] DBA aprobó la creación de esquemas  
- [ ] Seguridad revisó exposición de datos  
- [ ] Cada esquema tiene propietario correcto  

---

# 2. PROHIBICIONES ESTRICTAS (se deben auditar)

- [ ] **No existen objetos SATÉLITE en CORE**  
- [ ] **No existen accesos directos desde SATÉLITE a CORE**  
- [ ] **No existen GRANTs directos hacia CORE desde usuarios SAT**  
- [ ] **No existen sinónimos públicos**  
- [ ] **No existen tablas temporales SAT en el CORE**  
- [ ] **No existen vistas creadas por SAT en el CORE**  
- [ ] **No existen usuarios compartidos entre aplicaciones**  
- [ ] **No existen cadenas de conexión directas al CORE**  

---

# 3. VALIDACIÓN DEL FLUJO ÚNICO PERMITIDO  
**SATÉLITE → INTERFAZ → CORE**

- [ ] Todas las consultas a datos del CORE pasan por el esquema INTERFAZ  
- [ ] No existe ninguna ejecución SQL en código que referencie `CORE.` directamente  
- [ ] Las herramientas SAST/grep detectan intentos de acceso directo prohibido  
- [ ] Los paquetes del CORE no son invocados directamente desde SATÉLITE  

---

# 4. OBJETOS DE INTERFAZ (Vistas, Paquetes, Funciones)

## 4.1 Vistas de interfaz
- [ ] Vistas sólo exponen columnas autorizadas  
- [ ] Datos sensibles están enmascarados si corresponde  
- [ ] Los filtros obligatorios están aplicados  
- [ ] No existen joins complejos en interfaz que deberían estar en CORE  

## 4.2 Paquetes WRAPPER
- [ ] Validan parámetros de entrada  
- [ ] No exponen tipos internos del CORE  
- [ ] Manejan errores sin filtración de información  
- [ ] Documentación completa del contrato expuesto  

## 4.3 Revisiones obligatorias
- [ ] Arquitectura revisó objetos de interfaz  
- [ ] Seguridad revisó exposición de datos  
- [ ] DBA verificó performance y permisos  

---

# 5. USUARIOS, CREDENCIALES Y PERMISOS

## 5.1 Usuarios por aplicación y ambiente
- [ ] Cada aplicación tiene usuarios exclusivos por ambiente  
- [ ] No existen usuarios compartidos entre aplicaciones  
- [ ] Credenciales almacenadas únicamente en el gestor de secretos  

## 5.2 GRANTs permitidos
- [ ] SATÉLITE sólo recibe:  
  - [ ] `GRANT SELECT` sobre vistas de INTERFAZ  
  - [ ] `GRANT EXECUTE` sobre paquetes de INTERFAZ  

## 5.3 GRANTs prohibidos (se deben verificar)
- [ ] Ningún GRANT de SELECT/INSERT/UPDATE/DELETE sobre tablas CORE  
- [ ] Ningún GRANT de EXECUTE sobre paquetes del CORE  
- [ ] Ningún GRANT de SELECT ANY TABLE o permisos amplios  
- [ ] Ningún GRANT a PUBLIC  

---

# 6. VALIDACIONES EN CI/CD

- [ ] Pipeline verifica que no existen queries directas hacia CORE  
- [ ] Pipeline bloquea GRANTs no permitidos  
- [ ] Pipeline valida que cadenas de conexión apunten al esquema SATÉLITE  
- [ ] Pipeline valida versiones de scripts DDL/DML  
- [ ] Pipeline almacena evidencia de cambios en base de datos  
- [ ] El despliegue ejecuta scripts sólo desde carpeta de SAT/INT según corresponda  

---

# 7. AUDITORÍA DE ACCESOS Y OPERACIONES

- [ ] Auditoría habilitada en base de datos  
- [ ] Se registran intentos de acceso a CORE desde SATÉLITE  
- [ ] Se registran accesos legítimos a INTERFAZ  
- [ ] Reportes de auditoría disponibles para Seguridad TICs  
- [ ] Periodo de retención ≥ 12 meses en PROD  

---

# 8. VALIDACIONES DE QA

## 8.1 Validación funcional
- [ ] Consultas de la app funcionan sólo vía INTERFAZ  
- [ ] No se rompe integridad del CORE  
- [ ] Los paquetes wrapper funcionan correctamente  

## 8.2 Validación técnica
- [ ] Queries del SAT no contienen referencias a `CORE.`  
- [ ] No se exponen columnas prohibidas  
- [ ] Los datos sensibles están debidamente enmascarados  
- [ ] Vistas restringen acceso a filas según reglas corporativas  

---

# 9. PRUEBAS OPERACIONALES EN PRODUCCIÓN / PRE-PROD

- [ ] Monitoreo detecta accesos a esquemas  
- [ ] Alertas configuradas ante accesos no autorizados  
- [ ] Auditoría activa para eventos sospechosos  
- [ ] Performance validado en objetos de INTERFAZ  

---

# 10. ERRORES CRÍTICOS QUE IMPIDEN PROMOVER

- [ ] Existe un acceso directo SAT→CORE  
- [ ] Existen objetos SAT dentro del CORE  
- [ ] Existen GRANTs directos hacia CORE  
- [ ] Existen sinónimos públicos  
- [ ] Falta auditoría de BD  
- [ ] Usuario SAT comparte credenciales entre ambientes  
- [ ] Paquetes de interfaz no validan parámetros  
- [ ] Vistas exponen datos sensibles sin enmascarar  
- [ ] Pipeline no valida accesos a BD  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-08 — SEGMENTACIÓN DE ESQUEMAS Y ACCESO A DATOS
