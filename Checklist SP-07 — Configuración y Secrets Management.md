# CHECKLIST SP-07 — CONFIGURACIÓN Y SECRETS MANAGEMENT  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist verifica el cumplimiento de la Sub-Política SP-07 y del Manual Técnico MT-07.  
Debe completarse antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD.

---

# 1. SEPARACIÓN ENTRE CÓDIGO Y CONFIGURACIÓN

- [ ] Ninguna configuración sensible está incluida en el código  
- [ ] El proyecto sigue el principio 12-factor (configuraciones externas)  
- [ ] No existen valores de configuración hardcodeados en código fuente  
- [ ] Todos los parámetros están externalizados en variables de entorno o gestor de secretos  

---

# 2. CLASIFICACIÓN DE CONFIGURACIONES

## 2.1 Configuraciones no sensibles
- [ ] Variables no sensibles documentadas  
- [ ] Variables no sensibles definidas por ambiente  
- [ ] Variables no sensibles gestionadas correctamente en CI/CD  

## 2.2 Configuraciones sensibles (secretos)
- [ ] Todos los secretos están en el gestor de secretos corporativo  
- [ ] No existen secretos en archivos `.env` sin cifrado  
- [ ] No existen secretos en repositorios SVN  
- [ ] No existen secretos en logs del pipeline  
- [ ] Se verifica acceso seguro mediante tokens temporales  

---

# 3. USO DEL GESTOR DE SECRETOS CORPORATIVO

- [ ] El sistema obtiene secretos dinámicamente en tiempo de arranque  
- [ ] No se almacenan secretos en disco en servidores o containers  
- [ ] Se utilizan políticas de acceso RBAC por aplicación y ambiente  
- [ ] Cada aplicación tiene su propio conjunto de secretos  
- [ ] Se audita cada acceso al gestor de secretos  
- [ ] Se registra quién accede, cuándo, desde dónde y a qué secreto  

---

# 4. ROTACIÓN DE SECRETOS

- [ ] Existe política de rotación definida y aplicada  
- [ ] Rotación de contraseñas realizada según calendario  
- [ ] No existen secretos expirados en ambientes productivos  
- [ ] La aplicación soporta recarga de secretos sin downtime  
- [ ] Cambios de secretos fueron validados por QA  

---

# 5. CONFIGURACIONES POR AMBIENTE

- [ ] Existe configuración independiente para DEV, QA, UAT, PRE-PROD y PROD  
- [ ] No se comparten secretos entre ambientes  
- [ ] No existen configuraciones de otros ambientes almacenadas por error  
- [ ] El pipeline asegura que las configuraciones de cada ambiente están correctamente aplicadas  

---

# 6. VALIDACIONES AUTOMÁTICAS EN CI/CD

- [ ] Validador in-house detecta secretos expuestos  
- [ ] CI/CD falla si se encuentra un secreto en cualquier archivo del repositorio  
- [ ] CI/CD valida la consistencia de variables de entorno  
- [ ] CI/CD valida formato y estructura de configuraciones  
- [ ] Logs del pipeline no incluyen secretos filtrados  

---

# 7. CERTIFICADOS Y CRIPTOGRAFÍA

- [ ] Certificados almacenados únicamente en gestor de secretos  
- [ ] Ninguna clave privada existe en repositorios  
- [ ] Certificados productivos no están disponibles en ambientes inferiores  
- [ ] Se valida vigencia de certificados en monitoreo  
- [ ] La aplicación carga certificados en forma segura  

---

# 8. CONTROL DE ACCESO Y PERMISOS

- [ ] Roles de acceso al gestor de secretos definidos por aplicación  
- [ ] No existen accesos Global Read o permisos amplios  
- [ ] Accesos a secretos están auditados  
- [ ] Accesos a secretos están limitados por ambiente  
- [ ] No se comparte usuario para acceder a secretos entre apps diferentes  

---

# 9. ERRORES CRÍTICOS QUE IMPIDEN PROMOVER

- [ ] Secretos encontrados en repositorio  
- [ ] Secretos encontrados en archivos `.env` no cifrados  
- [ ] Secretos encontrados en código (variables hardcodeadas)  
- [ ] Secretos encontrados en logs del pipeline  
- [ ] Configuraciones sensibles sin gestor de secretos  
- [ ] Certificados privados en repositorios  
- [ ] Rotación de secretos vencida  
- [ ] Permisos amplios o roles inadecuados en gestor de secretos  

---

# 10. AUDITORÍA Y TRAZABILIDAD

- [ ] Cambios de configuración tienen aprobación formal  
- [ ] Reglas de acceso al gestor de secretos auditadas  
- [ ] Todos los cambios de secretos están registrados  
- [ ] La auditoría puede reconstruir el estado de configuraciones históricas  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-07 — CONFIGURACIÓN Y SECRETS MANAGEMENT
