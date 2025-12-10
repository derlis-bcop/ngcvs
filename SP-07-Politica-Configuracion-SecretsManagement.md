# SP-07 — POLÍTICA DE CONFIGURACIÓN Y SECRETS MANAGEMENT  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos obligatorios para la gestión segura, centralizada y auditada de configuraciones, credenciales, claves, certificados y secretos utilizados por los sistemas administrados por la Gerencia de TICs.  
El propósito es prevenir fugas de información, accesos indebidos, configuraciones inseguras y riesgos operativos derivados del manejo inadecuado de secretos.

## 2. Alcance
Aplica obligatoriamente a:
- Aplicaciones backend, frontend, móviles, APIs, microservicios, contenedores y servicios batch.  
- Scripts, integraciones, pipelines CI/CD y automatizaciones internas.  
- Cualquier componente que requiera credenciales, claves o configuraciones sensibles.

Tiene **carácter normativo interno** dentro de la Gerencia TICs.

## 3. Definiciones
- **Secretos:** Claves, contraseñas, tokens, certificados, llaves API, conexiones, llaves privadas y cualquier información sensible.  
- **Gestor de Secretos:** Plataforma corporativa aprobada (Vault u otro equivalente).  
- **Configuración Externa:** Parámetros de sistema definidos fuera del código fuente, cargados por ambiente.  
- **Rotación:** Proceso de cambiar periódicamente credenciales y claves.

## 4. Principios Normativos
1. **Ningún secreto puede almacenarse en código**, repositorios, logs o archivos de configuración simples.  
2. **Todos los secretos deben gestionarse mediante un gestor de secretos corporativo.**  
3. **Toda configuración sensible debe segregarse por ambiente.**  
4. **Toda lectura de secretos debe ser auditada y trazable.**  
5. **Los secretos deben rotarse periódicamente y nunca reutilizarse entre ambientes.**

## 5. Requisitos Técnicos Obligatorios

### 5.1 Almacenamiento de Secretos
Está totalmente prohibido:
- Incluir secretos en repositorios SVN  
- Incluir secretos en archivos `.env` sin cifrado  
- Exponer secretos en pipelines o logs  
- Compartir secretos entre desarrolladores  

Todo secreto debe residir en:
- Gestor de secretos aprobado  
- Almacenamiento cifrado y auditado  
- Estructuras organizadas por aplicación y ambiente

### 5.2 Acceso a Secretos
- Debe implementarse el **principio de menor privilegio**.  
- Los accesos deben ser otorgados por rol y revocados al finalizar proyectos.  
- El acceso debe realizarse mediante:  
  - Tokens temporales  
  - Roles dinámicos  
  - Mecanismos auditables  

Prohibido distribuir claves estáticas de larga duración.

### 5.3 Configuración por Ambiente
Cada ambiente (DEV, QA, UAT, PRE-PROD, PROD) debe poseer:
- Su propia configuración  
- Sus propios secretos  
- Sus propias claves y certificados  

Reutilizar configuraciones entre ambientes está prohibido.

### 5.4 Rotación de Secretos
Todo secreto debe rotarse:
- Periódicamente según criticidad  
- Tras detectar incidentes  
- Tras salida de personal  
- Tras compromisos o sospechas de fuga  

El pipeline debe ser capaz de operar sin interrupción durante la rotación.

### 5.5 Uso de Certificados y Cifrado
- Los certificados deben ser emitidos por la CA institucional.  
- Deben renovarse antes de la fecha de expiración.  
- Debe utilizarse TLS obligatorio para todas las comunicaciones sensibles.  
- Las claves privadas deben almacenarse exclusivamente en el gestor de secretos.

### 5.6 Validación en CI/CD
Los pipelines deben comprobar que:
- No existen secretos incrustados en código  
- Los repositorios no contienen archivos inseguros  
- Las credenciales provienen del gestor de secretos  
- Las configuraciones son por ambiente  

Cualquier incumplimiento genera fallo crítico del pipeline.

### 5.7 Auditoría de Accesos
El gestor de secretos debe registrar:
- Quién accedió  
- Qué secreto solicitó  
- Desde qué sistema  
- Cuándo ocurrió  
- Resultado de la operación (éxito/fallo)

La auditoría debe conservarse según lineamientos de Seguridad.

## 6. Lineamientos Operativos

### 6.1 Desarrollo
- Separar configuraciones del código.  
- Declarar parámetros configurables en documentación técnica.  
- Utilizar variables de entorno solo si son cargadas por el gestor de secretos.  
- Reportar secretos expuestos accidentalmente.

### 6.2 QA
- Validar que la aplicación funcione con secretos reales de QA.  
- Verificar ausencia de secretos en repositorio.  
- Registrar evidencias en CI/CD.  
- Confirmar integridad de configuraciones por ambiente.

### 6.3 Producción/Infraestructura
- Administrar el gestor de secretos.  
- Ejecutar rotación programada.  
- Configurar accesos seguros para despliegues automatizados.  
- Auditar uso y expiración de certificados.

### 6.4 Seguridad Informática
- Definir tiempos de rotación obligatoria.  
- Revisar accesos indebidos.  
- Auditar prácticas de configuración y manejo de secretos.  
- Emitir recomendaciones y rechazar implementaciones inseguras.

## 7. Controles Normativos
Ningún sistema podrá avanzar a PRE-PROD o PROD si:
- Contiene secretos en código fuente  
- Usa archivos `.env` sin cifrado  
- No implementa segregación por ambiente  
- No utiliza el gestor de secretos corporativo  
- No supera validaciones de CI/CD  
- No posee configuración auditada y documentada  

Producción y Seguridad poseen **poder de veto técnico normativo**.

## 8. Anexos
- Anexo A: Estructura recomendada de configuraciones por ambiente  
- Anexo B: Tipología de secretos y tratamiento obligatorio  
- Anexo C: Ejemplos de integraciones con gestor corporativo  
