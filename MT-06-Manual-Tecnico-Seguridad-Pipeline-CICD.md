# MT-06 — MANUAL TÉCNICO DE SEGURIDAD DEL PIPELINE CI/CD  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-06 Política de Seguridad del Pipeline CI/CD**

---

# 1. Propósito Operativo
Este manual establece cómo implementar un pipeline CI/CD seguro, auditado, inmutable y controlado.  
El objetivo es proteger la integridad del software, evitar manipulaciones, prevenir fugas de secretos y asegurar trazabilidad completa de cada despliegue, desde el commit en SVN hasta la puesta en producción.

---

# 2. Principios Fundamentales

## 2.1 El pipeline es la única vía de construcción y despliegue
- No puede existir **nada construido manualmente**.  
- No se debe subir binarios manuales.  
- No se permite modificar artefactos en ningún ambiente.

## 2.2 Inmutabilidad del pipeline
- La configuración del pipeline debe estar versionada.  
- Todo cambio en el pipeline requiere aprobación y debe quedar auditado.

## 2.3 Seguridad por diseño
- Principio de **mínimo privilegio**.  
- Ningún runner debe tener permisos excesivos.  
- Todos los secretos deben estar externalizados.

---

# 3. Seguridad de la Infraestructura del Pipeline

## 3.1 Runners / Agents
Requisitos obligatorios:
- Sistema operativo actualizado  
- Endurecimiento (hardening) aplicado  
- Acceso restringido por firewall  
- No deben almacenar credenciales en disco  
- No deben exponerse a internet salvo repositorios autorizados  

### 3.1.1 Runners dedicados por ambiente
- Producción → runners aislados  
- QA/DEV → runners sin acceso a credenciales críticas

## 3.2 Restricción de permisos
- Los runners solo ejecutan tareas necesarias  
- No deben tener acceso a bases de datos  
- No deben tener acceso directo a servidores productivos

---

# 4. Gestión Segura de Secretos

## 4.1 Prohibiciones estrictas
❌ No almacenar secretos en:  
- Código fuente  
- Variables del pipeline en texto plano  
- Archivos `.env` dentro del repositorio  
- Logs del pipeline  

## 4.2 Uso del gestor de secretos (Vault o equivalente)
Todos los secretos deben ser:
- Almacenados de forma cifrada  
- Accedidos mediante tokens temporales  
- Rotados periódicamente  
- Auditados

Ejemplo de lectura segura:

\```
vault kv get secrets/database | jq -r '.data.password'
\```

---

# 5. Análisis de Seguridad Obligatorios en CI/CD

Cada pipeline debe incluir:

## 5.1 SAST (Static Application Security Testing)
Herramientas permitidas:
- SonarQube  
- Bandit (Python)  
- eslint-security (Node.js)  

## 5.2 Análisis de Dependencias
- OWASP Dependency Check  
- pip-audit  
- npm audit  
- Trivy para contenedores  

## 5.3 Linting y validadores in-house
- Reglas de estilo  
- Reglas anti-secreto  
- Reglas anti-hardcode  

## 5.4 Escaneo de Imágenes de Contenedores
Obligatorio si el proyecto produce imágenes Docker.

Ejemplo:

\```
trivy image myapp:1.0.0 > trivy-report.txt
\```

Criticidades bloquean el pipeline automáticamente.

---

# 6. Evidencias del Pipeline

El pipeline debe **producir y almacenar evidencias obligatorias**, incluyendo:

- Hash del artefacto  
- SBOM completo  
- Resultados de SAST  
- Resultados de análisis de dependencias  
- Resultados de pruebas unitarias e integración  
- Logs del pipeline  
- Aprobaciones y responsables  
- Enlace directo al commit SVN  

Todas estas evidencias deben quedar archivadas junto al artefacto final.

---

# 7. Integridad del Artefacto

## 7.1 Hash obligatorio
\```
sha256sum artefacto.jar > artefacto.jar.sha256
\```

## 7.2 Validación automática
\```
sha256sum -c artefacto.jar.sha256
\```

Cualquier discrepancia → fallo crítico → pipeline se detiene.

## 7.3 Firmas opcionales (recomendado)
Firmar con clave institucional para fortalecer integridad.

---

# 8. Promoción Controlada entre Ambientes

## 8.1 Reglas obligatorias
- La promoción a QA, UAT, PRE-PROD y PROD **solo puede ocurrir desde el pipeline**.  
- No se recompila el código.  
- No se modifica configuración manualmente.  
- No se permite sobrescribir artefactos.

## 8.2 Requerimientos de aprobación
- Aprobación de QA para UAT  
- Aprobación dual (QA + Producción) para PRE-PROD  
- Aprobación de Producción + Seguridad para PROD  

Todo debe quedar registrado con nombre, fecha, hora y motivo.

---

# 9. Políticas de Auditoría

## 9.1 Auditoría trimestral
Seguridad y Producción revisarán:
- Cambios en pipelines  
- Configuración de runners  
- Manejo de secretos  
- Integridad del proceso  

## 9.2 Auditoría de logs
Todos los logs del pipeline deben ser:
- Inmutables  
- Almacenados por mínimo 12 meses  
- Accesibles para auditoría interna  

---

# 10. Errores Comunes y Mitigaciones

| Error | Consecuencia | Mitigación |
|-------|--------------|------------|
| Subir binarios manualmente | Artefacto no confiable | Bloquear publish manual |
| Guardar secretos en código | Violación crítica | Validadores anti-secreto |
| Dar permisos excesivos al runner | Riesgo de movimiento lateral | Principio de mínimo privilegio |
| No ejecutar SAST | Fallas explotables en producción | Pipeline obligatorio |
| No guardar evidencias | No trazabilidad | Evidencias automáticas |

---

# 11. Ejemplo de Pipeline Seguro (GitLab YAML)

\```
stages:
  - validate
  - build
  - test
  - security
  - package
  - deploy

validate:
  script:
    - ./validator_inhouse lint
    - ./validator_inhouse secrets
  artifacts:
    expire_in: 1 week

security:
  script:
    - trivy image myapp:latest > trivy.txt
    - pip-audit > audit.txt
  artifacts:
    paths:
      - trivy.txt
      - audit.txt

build:
  script:
    - mvn clean package
    - sha256sum target/app.jar > app.jar.sha256

package:
  script:
    - python sbom_generator.py > sbom.json
  artifacts:
    paths:
      - sbom.json
      - app.jar
      - app.jar.sha256

deploy_prod:
  stage: deploy
  when: manual
  script:
    - ansible-playbook deploy.yml
  environment: production
\```

---

# 12. Buenas Prácticas

- Mantener pipelines simples, modulares y legibles  
- Registrar cada cambio en el pipeline  
- Exigir revisiones por pares para cambios críticos  
- Evitar lógica compleja en scripts inline  
- Rotar tokens de acceso regularmente  

---

# FIN DEL MANUAL MT-06
