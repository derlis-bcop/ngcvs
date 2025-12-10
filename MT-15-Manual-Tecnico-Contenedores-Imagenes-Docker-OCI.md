# MT-15 — MANUAL TÉCNICO DE CONTENEDORES E IMÁGENES (DOCKER / OCI)  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-15

---

# 1. Propósito Operativo
Este manual define los estándares técnicos obligatorios para:

- Construcción segura de imágenes Docker/OCI  
- Uso adecuado de Dockerfile  
- Lineamientos de seguridad, performance y reproducibilidad  
- Estándares para publicación en el registry corporativo  
- Integración en pipelines CI/CD  
- Cumplimiento de NGCVS  

---

# 2. Estándares de Construcción de Imágenes

## 2.1 Imágenes base permitidas
- Deben provenir del registry corporativo  
- Deben tener versiones explícitas (`X.Y.Z`)  
- No se permite `latest`  

## 2.2 Multi-stage builds

Ejemplo:

\```
FROM node:18 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:1.23-alpine
COPY --from=build /app/dist /usr/share/nginx/html
\```

---

# 3. Seguridad del Dockerfile

Reglas obligatorias:

- No usar `USER root`  
- No usar `ADD` salvo necesidad de extracción  
- No incluir herramientas de debugging en PROD  
- No incluir secretos ni tokens  
- No descargar recursos externos en runtime  

Ejemplo incorrecto:

\```
RUN curl http://externo.com/archivo.sh | sh
\```

Ejemplo correcto:

\```
COPY scripts/instalador.sh /tmp/
RUN /tmp/instalador.sh
\```

---

# 4. Gestión de Secrets

### NUNCA dentro de imágenes  
- Variables peligrosas (`DB_PASSWORD`, `TOKEN`) prohibidas  
- Archivos `.env` prohibidos  
- Secrets deben venir del gestor corporativo en **runtime**  

---

# 5. Hardening de Imágenes

Checklist:

- Usuario no root  
- Capas mínimas  
- Read-only filesystem  
- No abrir puertos no utilizados  
- No incluir shells innecesarios  
- Firmado opcional de imágenes  

---

# 6. Optimización de Imágenes

- Minimizar layers  
- Usar Alpine o distribuciones mínimas cuando sea viable  
- Eliminar caches de build  
- Mantener tamaño final bajo el límite institucional  

Ejemplo:

\```
RUN apk add --no-cache bash
\```

vs incorrecto:

\```
RUN apk add bash
\```

---

# 7. Publicación y Registro

Todas las imágenes deben ser enviadas al registry corporativo:

\```
docker tag app:1.0.0 registry.interno/app:1.0.0
docker push registry.interno/app:1.0.0
\```

Versionado obligatorio:
- `MAJOR.MINOR.PATCH`
- Sin postfix ambiguos (`latest`, `stable`, etc.)

---

# 8. Escaneo de Vulnerabilidades

Requisito CI/CD obligatorio:

\```
trivy image registry.interno/app:1.0.0 --exit-code 1
\```

Si existen CVEs críticas → **pipeline falla**.

---

# 9. SBOM (Software Bill of Materials)

SBOM obligatorio:

- Generado en CI/CD  
- Formato CycloneDX o SPDX  
- Almacenado con la imagen  
- Validado en auditorías  

---

# 10. Integración con Kubernetes (Opcional / Recomendado)

Requisitos:

- Liveness y readiness probes  
- Recursos (`requests` / `limits`) obligatorios  
- SecurityContext restrictivo:  
  \```
  runAsUser: 1000  
  readOnlyRootFilesystem: true  
  allowPrivilegeEscalation: false  
  \```

---

# 11. Errores frecuentes

- Imagen pesada (>1GB)  
- Uso de root  
- Secretos embebidos  
- Instalar paquetes innecesarios  
- Publicar imágenes fuera del registry  
- Usar `latest`  

---

# FIN DEL MANUAL TÉCNICO MT-15
