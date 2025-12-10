# SP-15 — POLÍTICA DE CONTENEDORES E IMÁGENES (DOCKER / OCI)  
**Sub-Política Técnica — Nivel 1**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Dependiente de: NGCVS (Nivel 0)

---

# 1. Objetivo
Establecer las directrices técnicas y normativas para la creación, mantenimiento, seguridad, publicación y operación de imágenes de contenedores Docker/OCI en toda la Gerencia de TICs.  
Esta política garantiza la integridad, reproducibilidad, seguridad, trazabilidad y cumplimiento regulatorio de los sistemas que utilizan contenedores.

---

# 2. Alcance
Esta Sub-Política aplica a:

- Todos los proyectos que construyan imágenes Docker u OCI.  
- Aplicaciones internas, externas, legadas, nuevas o en evolución.  
- Procesos de CI/CD que generen, empaquen, escaneen o publiquen contenedores.  
- Equipos de Desarrollo, QA, Arquitectura, Seguridad y Producción/Infraestructura.  
- Todos los ambientes: DEV, QA, UAT, PRE-PROD y PROD.

---

# 3. Carácter Normativo
Esta Sub-Política es de **cumplimiento obligatorio** dentro de la Gerencia de TICs.  
Ningún contenedor podrá ser promovido hacia ambientes superiores si no cumple **todos** los requisitos establecidos en SP-15 y su Manual Técnico (MT-15).  
Producción e Infraestructura mantienen **poder de veto**.

---

# 4. Definiciones

### Contenedor  
Unidad mínima de ejecución aislada que empaqueta una aplicación y sus dependencias.

### Imagen Docker/OCI  
Plantilla inmutable a partir de la cual se instancian contenedores.

### Registry Corporativo  
Repositorio oficial donde deben almacenarse todas las imágenes aprobadas.

### Multi-stage build  
Técnica de optimización de Dockerfile que permite reducir tamaño y mejorar seguridad.

### SBOM (Software Bill of Materials)  
Inventario completo de dependencias incluidas en una imagen.

---

# 5. Principios Normativos

## 5.1 Seguridad por defecto
Todas las imágenes deben diseñarse con el principio de **menor superficie de ataque**:

- Usuario no root  
- Filesystem readonly cuando sea posible  
- Sin herramientas administrativas innecesarias  
- Sin shells ni compiladores salvo en la etapa de build

## 5.2 Inmutabilidad
Una imagen aprobada:

- No debe modificarse manualmente  
- Solo puede ser generada vía CI/CD  
- Debe ser estrictamente reproducible

## 5.3 Reproducibilidad determinística
El mismo Dockerfile, con la misma versión de dependencias y del código, debe producir la **misma imagen bit-a-bit**.

## 5.4 Minimización
Las imágenes deben contener solamente lo esencial para ejecutar la aplicación.

## 5.5 Integridad
Todas las imágenes deben contar con:

- Hash SHA-256  
- SBOM generado en CI/CD  
- Escaneo de vulnerabilidades aprobado  
- Evidencias de build completas

---

# 6. Políticas Obligatorias

## 6.1 Imágenes Base
1. Deben provenir del **registry corporativo** o repositorios oficiales verificados.  
2. Deben tener versiones fijas (`X.Y.Z`).  
3. Está prohibido el uso de:
   - `latest`  
   - imágenes sin firma  
   - imágenes sin soporte o EOL  

---

## 6.2 Dockerfile
El Dockerfile debe cumplir:

- Construcción **multi-stage** cuando corresponda  
- Prohibido incluir secretos  
- Prohibido descargar recursos externos en tiempo de build sin aprobación  
- No usar `ADD` salvo para extracción de archivos comprimidos  
- Combinar capas cuando sea posible para reducir tamaño  
- Documentar puertos expuestos, comando de entrada y usuario de ejecución

---

## 6.3 Seguridad de Imágenes
Toda imagen debe:

- Ser escaneada automáticamente (Trivy u homologado)  
- No poseer vulnerabilidades **críticas** ni **altas** sin excepción formal  
- Utilizar usuario **no root**  
- Tener permisos mínimos en filesystem  
- No incluir llaves privadas ni certificados

---

## 6.4 Secrets Management
Queda estrictamente prohibido:

- Incluir variables sensibles en la imagen  
- Incluir archivos `.env` en la imagen  
- Incluir claves o tokens en Dockerfile  
- Endurecer secretos en la imagen final  

Todos los secretos deben ser inyectados en **runtime** mediante el gestor corporativo.

---

## 6.5 Registry Corporativo
Toda imagen debe:

- Publicarse exclusivamente en el **registro institucional**  
- Usar versionado SEMVER  
- Mantener trazabilidad del build correspondiente  
- Contar con SBOM público para auditoría

---

## 6.6 CI/CD — Obligaciones
Los pipelines deben:

1. Construir imágenes en etapas determinísticas.  
2. Realizar escaneo de vulnerabilidades.  
3. Generar SBOM.  
4. Validar estructura del Dockerfile (hadolint u homólogos).  
5. Registrar evidencia completa del proceso.  
6. Impedir publicación si existe una vulnerabilidad crítica.

---

## 6.7 Despliegue y Operación
Cuando los contenedores se ejecuten en orquestadores (como Kubernetes), deben cumplir:

- `livenessProbe` obligatorio  
- `readinessProbe` obligatorio  
- `securityContext` restrictivo  
- `allowPrivilegeEscalation: false`  
- `readOnlyRootFilesystem: true` cuando sea posible  
- `runAsNonRoot: true`  

---

# 7. Roles y Responsabilidades

## Desarrollo
- Mantener Dockerfile  
- Garantizar seguridad del build  
- Minimizar tamaño y superficie de ataque  
- Asegurar reproducibilidad

## QA
- Validar comportamiento en contenedores  
- Asegurar que probes funcionen  
- Validar ausencia de secretos  

## Seguridad TICs
- Revisar vulnerabilidades  
- Aprobar excepciones  
- Validar cumplimiento criptográfico  

## Arquitectura
- Aprobar diseño de imágenes  
- Verificar buenas prácticas  
- Validar multi-stage build  

## Producción / Infraestructura
- Gestionar registry  
- Configurar políticas de retención  
- Ejecutar veto si se incumple la política  
- Operar contenedores bajo criterios institucionales  

---

# 8. Controles Normativos (Obligatorios)

- Ninguna imagen debe contener secretos  
- Ninguna imagen debe correr como root en PROD  
- Todas las imágenes deben contar con SBOM  
- Todas deben ser escaneadas en CI/CD  
- El hash de la imagen debe registrarse como evidencia  
- Las imágenes deben publicarse únicamente en el registry corporativo  
- Producción debe aprobar el despliegue final  

---

# 9. Vigencia y Actualización
Esta política será revisada **anualmente** por:

- Arquitectura TICs  
- Seguridad TICs  
- Producción e Infraestructura  

y será actualizada conforme a la evolución tecnológica y normativa.

---

# 10. Relación con Otros Documentos

- NGCVS (Nivel 0)  
- SP-01 Logging  
- SP-02 Monitoreo  
- SP-04 CI/CD  
- MT-15 Manual Técnico de Contenedores  
- Checklists de Nivel 3 asociados  

---

# FIN DEL DOCUMENTO — SP-15
