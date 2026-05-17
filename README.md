# Rizo_Arias-post2-u11

# PostContenido 2 — Sincronización Offline First con Observabilidad

Aplicaciones Móviles – Unidad 11  
Ingeniería de Sistemas – 2026

---

# Descripción

Este proyecto extiende el módulo de notas implementando una arquitectura Offline First con persistencia local y sincronización automática.

La aplicación continúa funcionando sin internet y sincroniza automáticamente al recuperar conectividad.

Tecnologías implementadas:

- Room
- WorkManager
- OpenTelemetry
- Hilt
- Coroutines
- MockWebServer
- Compose

---

# Objetivo

Implementar:

✅ Persistencia local

✅ Sincronización Offline First

✅ Retry automático

✅ Last Write Wins

✅ OpenTelemetry

✅ WorkManager

✅ Observabilidad

---

# Tecnologías Utilizadas

- Kotlin
- Room Database
- Hilt
- WorkManager
- OpenTelemetry
- MockWebServer
- Coroutines
- Flow
- Compose

---

# Arquitectura

```text
UI
 ↓
ViewModel
 ↓
Repository
 ↓
Room Database
 ↓
SyncWorker
 ↓
API
```

---

# Flujo Offline First

La aplicación sigue el siguiente flujo:

```text
Usuario crea nota sin internet
              ↓
Nota almacenada localmente
              ↓
Estado cambia a PENDING
              ↓
WorkManager espera internet
              ↓
Conectividad recuperada
              ↓
Worker ejecuta sincronización
              ↓
Datos enviados servidor
              ↓
Estado cambia a SYNCED
```

---

# Base de Datos

La entidad Note incorpora:

```kotlin
updatedAt:Long

syncStatus:SyncStatus
```

Estados:

```kotlin
enum class SyncStatus{

    PENDING,

    SYNCED,

    CONFLICT

}
```

---

# Resolución de Conflictos

Se implementó:

# Last Write Wins (LWW)

Regla:

```kotlin
if(server.updatedAt >
    local.updatedAt){

    // servidor reemplaza
}
```

Si el servidor tiene información más reciente, se reemplaza localmente.

Si la versión local es más nueva:

```kotlin
if(local.updatedAt >
    server.updatedAt){

    // conservar local
}
```

Esto evita pérdida de datos.

---

# Worker de Sincronización

Funcionalidades:

✅ obtiene cambios pendientes

✅ envía datos servidor

✅ marca sincronizados

✅ maneja errores

✅ aplica retry

✅ usa backoff exponencial

---

# Política Retry

Configuración:

```kotlin
BackoffPolicy.EXPONENTIAL
```

Tiempo:

```kotlin
15 segundos
```

Intentos:

```kotlin
3
```

---

# OpenTelemetry

Se instrumentó:

```kotlin
notes.sync
```

Atributos:

```kotlin
sync.attempt

sync.pending_count
```

Información registrada:

- inicio
- duración
- excepciones
- errores
- resultado

---

# Evidencia Logcat

Captura requerida:

```text
/screenshots/opentelemetry_logs.png
```

Debe visualizar:

```text
sync.attempt

sync.pending_count
```

---

# Evidencia WorkManager

Captura requerida:

```text
/ screenshots/workmanager.png
```

Debe mostrar:

```text
RUNNING

SUCCEEDED
```

---

# Capturas Requeridas

Agregar:

✅ Pending

✅ Synced

✅ Conflict

✅ Worker ejecutándose

✅ App Inspection

✅ OpenTelemetry Logcat

---

# Decisiones de Diseño

Room se utilizó para almacenamiento local persistente.

WorkManager fue seleccionado porque garantiza ejecución aunque la aplicación se cierre.

OpenTelemetry fue integrado para monitorear el proceso completo de sincronización.

Se implementó Last Write Wins por simplicidad y eficiencia.

---

# Checkpoints Verificados

✅ Migración correcta

✅ notas pendientes creadas

✅ sincronización automática

✅ worker ejecutado

✅ retry funcional

✅ LWW funcionando

✅ trazas visibles

---

# Cómo ejecutar

1. Clonar repositorio:

```bash
git clone URL_DEL_REPOSITORIO
```

2. Abrir Android Studio

3. Sincronizar Gradle

4. Ejecutar aplicación

5. Activar modo avión

6. Crear nota

7. Activar WiFi

8. Verificar sincronización automática

---

# Evidencias

---

## Ejecución de Fastlane en CI/CD

![fastlane_post2](evidencias/captura_fastlane_post2.png)

---

## Lanes configuradas en Fastlane

![lanes_post2](evidencias/captura_lanes_post2.png)

---

## Feature Flag en Firebase Remote Config

![remote_config_post2](evidencias/captura_remote_config_post2.png)

---

## Cambio de comportamiento en la app (Feature Flag ON/OFF)

![feature_flag_post2](evidencias/captura_feature_flag_post2.png)

---

## Validación de Conventional Commits

![commits_post2](evidencias/captura_commits_post2.png)
