# MT-03 — MANUAL TÉCNICO DE GESTIÓN DE DEPENDENCIAS Y LIBRERÍAS  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-03 Política de Dependencias y Librerías**

---

# 1. Propósito Operativo
Definir los procedimientos, configuraciones, validaciones y estándares técnicos para la correcta gestión de dependencias, módulos, librerías y frameworks utilizados en proyectos desarrollados o mantenidos por la Gerencia TICs.

El objetivo es garantizar:
- Seguridad  
- Trazabilidad  
- Reproducibilidad  
- Control de versiones  
- Reducción de riesgos por vulnerabilidades  

---

# 2. Principios Técnicos Fundamentales

## 2.1 Versiones Fijas (Pinning)
Todas las dependencias deben tener versión fija y explícita.  
Ejemplos:

```
numpy==1.25.2
spring-boot-starter-web:3.0.5
express@4.18.2
```

Prohibido:
- Rangos amplios (`>=`, `^`, `~`)  
- Usar “latest” o versiones flotantes  

## 2.2 Archivo de Lock Obligatorio
Cada tecnología debe generar un lockfile:

| Tecnología | Archivo Obligatorio |
|-----------|----------------------|
| Python | Pipfile.lock / poetry.lock |
| Node.js | package-lock.json |
| Java | mvnw + dependencias fijadas |
| Go | go.sum |

---

# 3. Gestión de Dependencias por Tecnología

## 3.1 Python (pip / pipenv / poetry)

### Instalación auditada
\```
pip install --require-hashes -r requirements.txt
\```

### requirements.txt con hashes
\```
requests==2.31.0 \
    --hash=sha256:4f45c9bd7aef \
    --hash=sha256:3d09adc0bcad
\```

---

## 3.2 Node.js (npm / yarn)

### Instalación determinística
\```
npm ci
\```

### Auditoría de vulnerabilidades
\```
npm audit --json
\```

---

## 3.3 Java (Maven / Gradle)

### Maven — análisis de dependencias
\```
mvn dependency:tree
\```

### Bloqueo de versiones con BOM
\```
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>3.0.5</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
\```

---

## 3.4 Go (Go modules)
\```
go mod tidy
go mod verify
\```

---

# 4. Auditoría de Vulnerabilidades

Toda dependencia debe pasar un escaneo automático en CI/CD.

Mínimos obligatorios:

| Severidad | Acción |
|-----------|--------|
| **Crítica (CVSS ≥ 9.0)** | Bloquea pipeline |
| **Alta (CVSS ≥ 7.0)** | Requiere excepción formal |
| Media/Baja | Registrar y corregir en sprint planificado |

Ejemplos de herramientas por lenguaje:

| Lenguaje | Herramienta |
|----------|-------------|
| Python | pip-audit, safety |
| Node.js | npm audit |
| Java | OWASP Dependency Check |
| Contenedores | Trivy, Grype |

---

# 5. Repositorios Autorizados

### 5.1 Permitidos
- Repositorios oficiales del lenguaje  
- Repositorios privados corporativos  
- Artefactories internos

### 5.2 Prohibidos
- Repositorios desconocidos  
- URLs directas a GitHub sin checksum  
- Dependencias copiadas manualmente  

---

# 6. SBOM (Software Bill of Materials)

## 6.1 Generación obligatoria
Cada pipeline debe producir un SBOM en:

- CycloneDX (preferido)  
- SPDX  

Ejemplo (CycloneDX JSON):

\```
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "components": [
    {
      "name": "requests",
      "version": "2.31.0",
      "hashes": [{"alg": "SHA-256", "content": "abc123"}]
    }
  ]
}
\```

## 6.2 Almacenamiento
Todo SBOM debe guardarse en:
- Artefacto adjunto del pipeline  
- Repositorio de evidencias  

---

# 7. Validaciones en CI/CD

Checklist de validaciones automáticas:

- Dependencias con versión fija  
- Archivo lock presente  
- Auditoría sin vulnerabilidades críticas  
- SBOM generado  
- Verificación de integridad (hash)  
- Dependencias transitivas revisadas  

---

# 8. Políticas de Actualización

## 8.1 Frecuencia
- Tecnologías críticas → mensual  
- Librerías estándar → trimestral  
- Frameworks mayores → semestral  

## 8.2 Condiciones obligatorias
- Pruebas unitarias completas  
- Pruebas de regresión funcional  
- Pruebas de integración si aplica  
- Validación en QA  
- Seguimiento de breaking changes  

---

# 9. Buenas Prácticas

- Preferir librerías oficiales y con soporte  
- Evitar dependencias excesivas (“dependency bloat”)  
- Revisar transitive dependencies  
- Mantener historial de versiones  
- Minimizar dependencias internas no documentadas  
- Revisar Release Notes antes de actualizar  

---

# 10. Advertencias y Errores Frecuentes

- ❌ Usar dependencias sin versión fija  
- ❌ Mantener dependencias EOL  
- ❌ Instalar dependencias manualmente  
- ❌ Borrar lockfiles  
- ❌ Ignorar auditorías automáticas  
- ❌ Usar librerías sin mantenimiento activo  

---

# 11. Ejemplo de Pipeline CI/CD con Validación de Dependencias

\```
stages:
  - dependencies
  - build

dependencies:
  script:
    - pip install pip-audit
    - pip-audit --strict
    - pip install -r requirements.txt
    - python -m sbom_generator > sbom.json
  artifacts:
    paths:
      - sbom.json
\```

---

# FIN DEL MANUAL MT-03
