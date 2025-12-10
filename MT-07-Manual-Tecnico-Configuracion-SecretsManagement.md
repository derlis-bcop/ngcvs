# MT-07 — MANUAL TÉCNICO DE CONFIGURACIÓN Y SECRETS MANAGEMENT  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-07 Política de Configuración y Secrets Management**

---

# 1. Propósito Operativo
Este manual define los lineamientos técnicos, buenas prácticas, patrones y procedimientos obligatorios para:

- Manejo seguro de configuraciones  
- Gestión centralizada de secretos  
- Prevención de filtración de credenciales  
- Uso adecuado del gestor de secretos corporativo  
- Desacoplamiento de la configuración del código  
- Cumplimiento de los controles de seguridad y auditoría  

---

# 2. Clasificación de Configuraciones y Secretos

## 2.1 Configuraciones no sensibles
Incluyen:
- Parámetros de negocio  
- Flags de funcionalidad  
- Parámetros no críticos  
- Timeouts  
- URLs públicas  

Pueden almacenarse en:
- Archivos por ambiente  
- Variables de entorno  
- ConfigMaps (en caso de orquestadores)  

## 2.2 Configuraciones sensibles (SIEMPRE SECRETOS)
Incluyen:
- Passwords  
- Tokens  
- API Keys  
- Certificados  
- Llaves privadas  
- JDBC URLs con credenciales  

Deben almacenarse **exclusivamente** en el gestor de secretos corporativo.

---

# 3. Buenas Prácticas de Configuración

## 3.1 Separación estricta entre código y configuración
Toda configuración debe residir **fuera** del código fuente.

Ejemplo prohibido:

\```
db_password = "1234"
\```

Ejemplo correcto:

\```
db_password = os.getenv("DB_PASSWORD")
\```

## 3.2 Configuración declarativa por ambiente
Cada ambiente (DEV, QA, UAT, PRE-PROD, PROD) requiere su propio set de configuraciones.

No se permite reutilizar configuraciones entre ambientes.

## 3.3 Carga de configuración en tiempo de arranque
La aplicación debe cargar toda su configuración durante el arranque y validarla.

Ejemplo en Python:

\```
import os

required = ["DB_HOST", "DB_PORT", "DB_USER", "DB_PASSWORD"]
for key in required:
    if not os.getenv(key):
        raise Exception(f"Missing required configuration: {key}")
\```

---

# 4. Gestión Segura de Secretos

## 4.1 Reglas absolutas (PROHIBIDO)
❌ Secretos en repositorios  
❌ Secretos en logs  
❌ Secretos en archivos `.env` no cifrados  
❌ Secretos en correos o chats  
❌ Secretos en imágenes Docker  

## 4.2 Uso del Gestor de Secretos Corporativo
Requisitos:

- Acceso mediante credenciales temporales  
- Auditoría obligatoria  
- Versiones de secretos  
- Revocación programada  
- Rotación periódica automática o manual  

Ejemplo de consulta:

\```
vault kv get secret/proyecto/app
\```

---

# 5. Rotación de Secretos

## 5.1 Motivos para rotar
- Cambio de personal  
- Fecha de vencimiento  
- Exposición accidental  
- Recomendación de auditoría  
- Cambios criptográficos globales  

## 5.2 Rotación transparente sin downtime
La aplicación debe soportar recarga de secretos sin caída.

Método recomendado:
- Rotar secretos → Pipeline actualiza → Aplicación recarga desde el gestor

---

# 6. Almacenamiento por Ambiente

Ejemplo de estructura:

\```
secret/
  proyecto/
    dev/
    qa/
    uat/
    preprod/
    prod/
\```

Nunca se debe compartir secretos entre ambientes.

---

# 7. Certificados y Criptografía

## 7.1 Certificados
- Emitidos por la CA institucional  
- Vigencia monitoreada  
- Renovados antes del vencimiento  

## 7.2 Llaves privadas
- Deben almacenarse únicamente en el gestor de secretos  
- Acceso restringido a roles autorizados  
- Prohibido exportarlas a disco  

---

# 8. Validaciones Automáticas en CI/CD

El pipeline debe:

- Detectar secretos expuestos  
- Impedir commits con secretos mediante validador in-house  
- Verificar que las variables provienen del gestor de secretos  
- Validar formato de archivos de configuración  
- Fallar si un secreto está en texto plano

Ejemplo de verificación:

\```
./validator_inhouse secrets --strict
\```

---

# 9. Ejemplos por Tecnología

## 9.1 Spring Boot
Variables externalizadas:

\```
spring.datasource.password=${DB_PASSWORD}
\```

## 9.2 Node.js
\```
const dbPass = process.env.DB_PASSWORD;
\```

## 9.3 Python
\```
import os
password = os.getenv("PASSWORD")
\```

---

# 10. Auditoría y Registro

El gestor de secretos debe registrar:

- Quién accedió  
- Desde dónde  
- Qué operación realizó  
- Qué secreto fue consultado  
- Fecha y hora  
- Resultado  

Auditoría mínima: **12 meses**.

---

# 11. Errores Comunes y Consecuencias

| Error | Riesgo | Consecuencia |
|-------|--------|--------------|
| Secretos en repositorio | Fuga de credenciales | Incidente crítico |
| Secretos en imágenes Docker | Exposición total del entorno | Despliegue bloqueado |
| No rotar secretos | Abuso prolongado | Riesgo operativo |
| Compartir secretos entre ambientes | Escalada lateral | Violación de normas |
| Secretos en logs | Fuga irreversible | Alta severidad |

---

# 12. Checklist de Validación Técnica (antes de despliegue)

- [ ] Ningún secreto en el repositorio  
- [ ] Configuración por ambiente documentada  
- [ ] Secrets cargados desde gestor corporativo  
- [ ] Rotación programada en el pipeline  
- [ ] Certificados actualizados  
- [ ] Auditoría del gestor activa  
- [ ] Variables de entorno correctamente mapeadas  
- [ ] Validación anti-secreto aprobada (CI/CD)  

---

# FIN DEL MANUAL MT-07
