# MT-04 — MANUAL TÉCNICO DE ARTEFACTOS, CI/CD Y VERSIONAMIENTO  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-04 Política de Artefactos, CI/CD y Versionamiento**

---

# 1. Propósito Operativo
Este manual detalla los estándares técnicos obligatorios para:

- Construcción reproducible de artefactos  
- Versionamiento semántico (SEMVER)  
- Integración total con CI/CD  
- Firma, hash e integridad  
- Publicación en repositorios corporativos  
- Automatización completa del despliegue  
- Evidencias obligatorias  
- Reglas de promoción entre ambientes  

Define **cómo implementar** lo que la política SP-04 exige normativamente.

---

# 2. Artefactos — Reglas Técnicas

## 2.1 ¿Qué es un artefacto válido?
Un artefacto es válido únicamente si cumple:

- Construido por CI/CD  
- Versionado con SEMVER  
- Incluye hash de integridad (SHA-256 o superior)  
- Se almacena en repositorio corporativo  
- Está registrado en evidencias de pipeline  
- Posee trazabilidad al commit (SVN revision)  

Artefactos construidos manualmente → **prohibidos**.

---

# 3. Versionamiento (SEMVER Obligatorio)

Formato:  
`MAJOR.MINOR.PATCH`

## 3.1 Significado
- **MAJOR:** Cambios incompatibles  
- **MINOR:** Nuevas funcionalidades compatibles  
- **PATCH:** Correcciones y mejoras menores  

## 3.2 Ejemplos válidos
- `1.0.0`  
- `2.1.4`  
- `3.0.0`  

## 3.3 Ejemplos no válidos
- `1.0`  
- `latest`  
- `5.x`  
- `release-2025`  

---

# 4. Cambios Documentados — CHANGELOG Obligatorio

Formato recomendado:

\```
## [2.1.0] — 2025-03-01
### Added
- Nuevo endpoint POST /clientes

### Fixed
- Corrección en validador de DNI

### Changed
- Actualización de módulo de notificaciones
\```

El CHANGELOG debe estar en el repositorio SVN junto al código fuente.

---

# 5. Construcción del Artefacto

## 5.1 Requisitos del proceso de build
Debe ser:

- Determinístico  
- Reproducible  
- Automatizado  
- Basado en scripts versionados  

Ejemplos:

### 5.1.1 Python
\```
python -m build
\```

### 5.1.2 Java (Maven)
\```
mvn clean package -DskipTests=false
\```

### 5.1.3 Node.js
\```
npm ci && npm run build
\```

---

# 6. Hash y Firma del Artefacto

## 6.1 Generación de hash
Obligatorio:

\```
sha256sum artefacto.jar > artefacto.jar.sha256
\```

## 6.2 Validación de hash
El pipeline debe comparar:

\```
sha256sum -c artefacto.jar.sha256
\```

Si el hash cambia → artefacto inválido → pipeline falla.

---

# 7. Repositorios de Artefactos

## 7.1 Ubicación obligatoria
Todo artefacto debe publicarse en:

- Repositorio privado corporativo (Nexus, Artifactory o equivalente)  
- Estructura:  
  - /proyecto  
  - /version  
  - /artefactos  
  - /sbom  
  - /evidencias  

## 7.2 Prohibiciones
- No se permite almacenar artefactos en:
  - Correos  
  - SharePoint  
  - Discos locales  
  - Servidores no autorizados  

---

# 8. Integración con CI/CD

El pipeline debe incluir al menos:

1. **Validación de código**  
2. **Pruebas unitarias**  
3. **Pruebas de integración**  
4. **Validación de dependencias**  
5. **SAST**  
6. **Generación del artefacto**  
7. **Hash del artefacto**  
8. **Generación de SBOM**  
9. **Publicación del artefacto**  
10. **Evidencias completas**  

---

# 9. Evidencias de CI/CD

El pipeline debe adjuntar:

- Logs de build  
- Reportes de seguridad  
- Resultados de pruebas  
- SBOM  
- Hash del artefacto  
- Información del commit  
- Tiempos de ejecución  
- Captura del versionamiento utilizado  

Sin estas evidencias → artefacto **NO puede promoverse**.

---

# 10. Reglas de Promoción entre Ambientes

## 10.1 Flujo obligatorio
`DEV → QA → UAT → PRE-PROD → PROD`

## 10.2 Promoción estricta del mismo binario
- No se recompila  
- No se modifica  
- No se parchea en ambiente  

## 10.3 Condiciones obligatorias para promover

| Requisito | DEV | QA | UAT | PRE-PROD | PROD |
|-----------|-----|----|-----|----------|-------|
| Pruebas unitarias | ✓ | ✓ | ✓ | ✓ | ✓ |
| Evidencias CI/CD | ✓ | ✓ | ✓ | ✓ | ✓ |
| Seguridad SAST | ✓ | ✓ | ✓ | ✓ | ✓ |
| Aprobación QA | — | ✓ | ✓ | ✓ | — |
| Aprobación Producción | — | — | — | ✓ | ✓ |
| Validación de rollback | — | — | — | ✓ | ✓ |

---

# 11. Despliegue Automatizado

## 11.1 Sin intervención manual
Formato prohibido:
- FTP  
- SSH manual  
- Copiar artefactos a mano  
- Modificación de configuración directamente en servidor  

## 11.2 Ejemplo de despliegue automatizado en CI/CD
\```
deploy:
  stage: deploy
  script:
    - ansible-playbook deploy.yml --extra-vars "version=$CI_COMMIT_TAG"
  environment: production
  when: manual
\```

---

# 12. Rollback Formal

Cada release debe incluir:

- Procedimiento de rollback  
- Validación del rollback en PRE-PROD  
- Evidencia de ejecución en pipeline  

Ejemplo:

\```
ansible-playbook rollback.yml --extra-vars "version=1.4.2"
\```

---

# 13. Auditoría y Control de Cambios

Producción debe registrar:

- Responsable del despliegue  
- Artefacto utilizado  
- Versiones previas  
- Fecha/hora  
- Hashes  
- Logs del pipeline  

Los auditores deben poder reconstruir la historia completa de despliegues.

---

# 14. Buenas Prácticas

- Minimizar tamaño de artefactos  
- Generar SBOM siempre  
- Evitar dependencias no utilizadas  
- Documentar breaking changes  
- Mantener consistencia entre ramas y tags  

---

# 15. Errores Frecuentes (y su mitigación)

- ❌ Build manual → **prohibido**  
- ❌ Artefacto sin hash → **inválido**  
- ❌ Cambiar configuración directamente en PROD → incidente crítico  
- ❌ No firmar artefactos → incumplimiento  
- ❌ No registrar evidencias → no puede promoverse  

---

# FIN DEL MANUAL MT-04
