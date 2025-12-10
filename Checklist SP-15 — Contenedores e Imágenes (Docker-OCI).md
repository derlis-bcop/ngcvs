# CHECKLIST SP-15 — CONTENEDORES E IMÁGENES (DOCKER / OCI)  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist valida el cumplimiento técnico, operativo y de seguridad de la Sub-Política SP-15 y del Manual Técnico MT-15.  
Es obligatorio antes de promover imágenes a QA, UAT, PRE-PROD y PROD.

---

# 1. BASE DE LA IMAGEN (IMAGEN PADRE)

- [ ] La imagen base proviene del registro corporativo  
- [ ] No se utiliza `latest` bajo ninguna circunstancia  
- [ ] La versión base es fija (`X.Y.Z`)  
- [ ] La imagen base tiene soporte vigente y no está EOL  
- [ ] Imagen base pasó escaneo de vulnerabilidades (CVE)  

---

# 2. DOCKERFILE — REGLAS OBLIGATORIAS

- [ ] Dockerfile versionado en SVN  
- [ ] Dockerfile no contiene secretos  
- [ ] Se utiliza usuario no root  
- [ ] Se definen permisos mínimos en filesystem  
- [ ] Existen capas limpias (RUN combinados correctamente)  
- [ ] Se utilizan `COPY` en lugar de `ADD` (excepto casos permitidos)  
- [ ] No se instalan herramientas innecesarias  
- [ ] Se eliminan caches, tmp y paquetes al final del build  

---

# 3. SEGURIDAD DE LA IMAGEN

- [ ] Imagen escaneada con Trivy u otra herramienta oficial  
- [ ] No existen vulnerabilidades críticas  
- [ ] Vulnerabilidades altas tienen excepción formal  
- [ ] No existen binarios sospechosos  
- [ ] No existe acceso root  
- [ ] No existen puertos expuestos sin necesidad  

---

# 4. SECRETS MANAGEMENT EN CONTENEDORES

- [ ] Ningún secreto está dentro de la imagen  
- [ ] Ningún archivo `.env` existe dentro de la imagen  
- [ ] Secrets se inyectan en runtime desde el gestor corporativo  
- [ ] No se almacenan claves privadas en el filesystem de la imagen  
- [ ] Variables de entorno no contienen información sensible  

---

# 5. REPRODUCIBILIDAD Y DETERMINISMO

- [ ] Build realizado exclusivamente por CI/CD  
- [ ] Dockerfile produce la misma imagen en cualquier ambiente  
- [ ] Hash del resultado se conserva (`sha256`)  
- [ ] SBOM generado y almacenado con la imagen  

---

# 6. PERFORMANCE Y OPTIMIZACIÓN

- [ ] Imagen final < tamaño máximo admitido institucionalmente  
- [ ] Capas optimizadas y ordenadas  
- [ ] Se utiliza multi-stage build cuando corresponde  
- [ ] No existe instalación de compiladores en la imagen final  
- [ ] Se validó tiempo de arranque (fast startup)  

---

# 7. REGISTRO DE IMÁGENES (REGISTRY)

- [ ] La imagen se publica únicamente en el registry corporativo  
- [ ] No se publican imágenes en repositorios externos  
- [ ] Versionado correcto `app:MAJOR.MINOR.PATCH`  
- [ ] Política de retención cumple normas (mín. 12 meses)  

---

# 8. DESPLIEGUE Y ORQUESTACIÓN (si aplica Kubernetes)

- [ ] `livenessProbe` configurado  
- [ ] `readinessProbe` configurado  
- [ ] Uso de recursos (`requests`/`limits`) definido  
- [ ] No existen privilegios elevados (`privileged: false`)  
- [ ] Root filesystem es readonly (`readOnlyRootFilesystem`)  
- [ ] No se monta `/var/run/docker.sock`  

---

# 9. CI/CD — VALIDACIONES AUTOMÁTICAS

- [ ] Escaneo automático de imagen  
- [ ] Generación de SBOM  
- [ ] Validación de Dockerfile mediante linters (hadolint)  
- [ ] Validación de seguridad automática  
- [ ] Evidencias archivadas  

---

# 10. ERRORES CRÍTICOS QUE IMPIDEN PROMOVER

- [ ] Secretos dentro de la imagen  
- [ ] Vulnerabilidades críticas sin corregir  
- [ ] Imagen `latest`  
- [ ] Usuario root  
- [ ] Falta liveness/readiness  
- [ ] Imagen no proviene del pipeline  
- [ ] Imagen publicada fuera del registry institucional  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de observaciones:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-15
