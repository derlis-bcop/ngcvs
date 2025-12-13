# SP-18 — POLÍTICA DE NO ADMISIÓN DE ARTEFACTOS NO OPERABLES  
**Sub-Política Técnica — Nivel 1**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Dependiente de: NGCVS — Norma General de Ciclo de Vida de Software

---

# 1. Objetivo

Establecer la prohibición absoluta de admitir, promover o desplegar artefactos que dependan de ejecución manual, procedimientos interactivos, ejecución desde CLI sin supervisión, envío manual al background, o que requieran intervención humana para recuperarse tras fallas operativas.  

Esta política garantiza que todos los artefactos desplegados en entornos institucionales sean:

- **Operables 24/7**  
- **Resistentes a fallos críticos**  
- **Autorreparables** (zero-touch recovery)  
- **Supervisables y auditables**  
- **Compatibles con la infraestructura de Producción e Infraestructura**  

---

# 2. Alcance

Aplicable a:

- Todos los sistemas nuevos, evolutivos o correctivos.  
- Aplicaciones backend, APIs, microservicios, batch, workers, ETL, gateways, integración.  
- Contenedores OCI, servicios SOA, módulos internos, scripts automatizados.  
- Artefactos en DEV, QA, UAT, PRE-PROD y PROD.  

Incluye desarrollos internos, externos, contratados o adquiridos.

---

# 3. Carácter Normativo

Esta política es **obligatoria dentro de la Gerencia de TICs**.  
No puede ser ignorada ni modificada sin aprobación de:

- Arquitectura  
- Seguridad TICs  
- Producción/Infraestructura  
- Gerencia de TICs  

Producción mantiene **poder de veto** para cualquier artefacto que incumpla esta política.

---

# 4. Definiciones

### Artefacto No Operable  
Cualquier ejecutable, script, proceso o programa que:

- Requiera comandos CLI manuales para iniciar.  
- Requiera ser enviado al background usando `&`, `nohup`, `screen`, `tmux` o equivalentes.  
- No cuente con un supervisor automático (systemd, supervisord, contenedor, scheduler institucional).  
- No reinicie automáticamente ante fallos.  
- No exponga health checks.  
- Requiera intervención humana para recuperarse tras errores.  

### Artefacto Operable (Permitido)  
Aquel que:

- Se ejecuta como **servicio persistente** con restart policies.  
- O se ejecuta como **contenedor OCI** con health checks y restart automático.  
- Posee **zero-touch recovery** (recuperación automática sin intervención humana).  
- Está integrado a monitoreo, métricas, logs estructurados y telemetría.  
- Tiene un mecanismo de despliegue reproducible y auditado por CI/CD.  

---

# 5. Políticas Obligatorias

## 5.1 Prohibición absoluta de artefactos no operables

Está **prohibido** entregar o desplegar artefactos que:

- Se inicien manualmente desde CLI.  
- Requieran uso de `nohup`, `screen`, `tmux` o similares.  
- Deban quedar ejecutándose como procesos “sueltos” sin supervisor.  
- No puedan reiniciarse automáticamente ante fallos.  
- No tengan mecanismo formal de despliegue.  
- No cuenten con health checks mínimos.  
- No expongan logs estructurados o métricas técnicas.  
- Requieran monitoreo manual o intervención humana para detectar caídas.  
- No estén preparados para **caídas de red**, **errores de base de datos**, **timeouts**, etc.  

Cualquier artefacto que incumpla estas condiciones debe ser **rechazado de inmediato**.

---

## 5.2 Exigencia obligatoria de ejecución como servicio persistente

Todo artefacto backend, worker, API o proceso continuo debe ejecutarse como:

- **Servicio systemd**, o  
- **Servicio supervisado por un orquestador institucional**, o  
- **Contenedor OCI con restart policies activas**, o  
- **Módulo dentro de una plataforma con supervising nativo (Ej: Airflow, Kafka Streams, Celery con supervisor, etc.)**

El servicio debe:

- Reiniciarse automáticamente ante fallos.  
- Iniciar automáticamente en arranque del servidor/contenedor.  
- Registrar su estado en monitoreo.  
- Poseer health checks funcionales (`/health`, `/live`, `/ready`).  

---

## 5.3 Exigencia obligatoria de ejecución en contenedores (cuando aplique)

Para aplicaciones modernas, APIs, microservicios y workers:

- Deben entregarse como **contenedores OCI**.  
- Deben incluir health checks.  
- Deben contar con restart automático (`always`, `on-failure`).  
- Deben incluir manejo de errores de infraestructura y red.  

Se prohíbe:

- Desplegar contenedores sin políticas de restart.  
- Contenedores que requieren ejecutar scripts internos manualmente.  
- Contenedores que no exponen servicios listos para supervisión.

---

## 5.4 Autorrestart obligatorio

Todo artefacto debe reiniciarse automáticamente ante fallos como:

- Caída de conexión a la base de datos.  
- Pérdida temporal de red.  
- Errores internos no fatales.  
- Saturación temporal de recursos.  
- Timeouts.  

Umbrales, políticas de retry y backoff serán definidos por Arquitectura.

---

## 5.5 Zero-Touch Recovery

El sistema debe recuperarse sin intervención humana, cumpliendo:

- Autorrestart del servicio.  
- Reconexión automática a la BD.  
- Reprocesamiento idempotente de tareas (si aplica).  
- Persistencia y consistencia garantizadas.  
- Supervisión automática en Producción.  

**Cualquier arquitectura que requiera intervención humana queda prohibida.**

---

## 5.6 Requisitos obligatorios adicionales

Todo artefacto debe:

- Estar completamente automatizado en su despliegue.  
- Contar con logs estructurados con trazabilidad (`trace_id`).  
- Exponer métricas técnicas y de producto.  
- Integrarse al monitoreo institucional.  
- Contener health checks listos para Producción.  
- Ser auditado por pipeline CI/CD (SP-04).  
- Contar con rollback automático documentado.  
- Ser reproducible desde cero.  
- No almacenar estados críticos en memoria sin persistencia externa.  
- No depender de procesos “huérfanos” ni de servicios iniciados manualmente.  

(*Puedes decidir más adelante retirar o ajustar algunos de estos puntos.*)

---

# 6. Excepciones Permitidas (Bajo Autorización Formal)

Se permiten excepciones **únicamente** cuando:

- Arquitectura apruebe la justificación técnica.  
- Seguridad TICs valide los riesgos.  
- Producción apruebe la operabilidad.  
- La Gerencia de TICs otorgue aprobación final.

Excepciones válidas (ejemplos):

1. Herramientas internas de uso esporádico  
2. Scripts administrativos controlados  
3. Procesos migratorios de corta duración  
4. Módulos legacy en plan de decomisión  

Las excepciones deben:

- Ser temporales  
- Tener fecha de expiración  
- Contar con procedimiento de reemplazo  

---

# 7. Roles y Responsabilidades

## Desarrollo  
- Diseñar servicios operables, autorreparables y supervisables.  
- Eliminar cualquier dependencia de CLI interactivo.  

## QA  
- Validar resiliencia y restart automático en ambientes controlados.  

## Arquitectura  
- Definir patrones, plantillas y requisitos estándares.  

## Seguridad TICs  
- Validar que ningún mecanismo de autorrestart comprometa información.  

## Producción/Infraestructura  
- Auditar operabilidad real.  
- Configurar supervisión, políticas de restart y monitoreo.  
- Vetar artefactos no operables.  

---

# 8. Controles Normativos

Un artefacto debe ser **rechazado** si:

- No funciona como servicio o contenedor supervisado.  
- No tiene restart automático.  
- Carece de health checks.  
- Requiere ejecución manual desde CLI.  
- No posee logs estructurados ni métricas.  
- Requiere intervención humana para recuperarse.  
- No integra monitoreo institucional.  

---

# 9. Relación con otros documentos

- NGCVS (Nivel 0)  
- SP-01 Logging  
- SP-02 Monitoreo  
- SP-05 Health Checks  
- SP-15 Contenedores OCI  
- SP-17 Métricas y Telemetría  
- MT-18 (Manual Técnico asociado)  
- Checklist SP-18 (Nivel 3)

---

# FIN DEL DOCUMENTO — SP-18
