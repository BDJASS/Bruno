# 01 · GET /SupervisorAdm — GetSupervisor

**Programa:** `RestADOSASysAdmin/AppServer/catsupervisorempleado.p`
**Procedimiento:** `GetSupervisor` · **Verbo:** `GET` · **Autor:** SIS10 — JASS

## 1. Descripción

Catálogo de **supervisores** (tabla `Supervisor`). Por cada supervisor resuelve el nombre del usuario ligado (`Supervisor.Iniciales` = `Usuario.Id-User`). Lo consume `GestionUsuariosAdministracion.razor` (campo Supervisor: búsqueda por clave, nombre o usuario). Lectura con `NO-LOCK`; ordenado por `Id-Sup`.

## 2. Endpoint

`GET http://192.0.1.14:8813/RestADOSASysAdmin/rest/RestADOSASysAdmin/SupervisorAdm`

## 3. Parámetros de entrada (Query String)

Ninguno.

## 4. Parámetros de salida

**`IdError`** · LOGICAL — `false` = consulta exitosa; `true` = ocurrió un error
**`Respuesta`** · CHARACTER — `"OK"` en éxito; `"Error: <mensaje>"` si falló (capturado por `CATCH`)
**`ttSupervisor`** · TABLE — Un registro por supervisor

## 5. Campos del response (ttSupervisor)

**`IdSup`** · INTEGER — Clave del supervisor (`Supervisor.Id-Sup`)
**`Iniciales`** · CHARACTER — Iniciales del empleado/usuario (`Supervisor.Iniciales` = `Usuario.Id-User`)
**`NomUsuario`** · CHARACTER — Nombre del usuario (`Usuario.Nom-Usuario`); vacío si no existe el usuario

## 6. Ejemplos de uso

Éxito (envoltorio del adaptador clásico):

```json
{
  "response": {
    "IdError": false,
    "Respuesta": "OK",
    "ttSupervisor": {
      "ttSupervisor": [
        { "IdSup": 3, "Iniciales": "olds", "NomUsuario": "OSCAR LESTER DELGADO SANCHEZ" }
      ]
    }
  }
}
```

Error:

```json
{
  "response": {
    "IdError": true,
    "Respuesta": "Error: <mensaje>"
  }
}
```

## 7. Notas técnicas

- **Solo lectura** — `NO-LOCK`, no bloquea la tabla `Supervisor`.
- Supervisor sin usuario ligado en `Usuario` regresa `NomUsuario` vacío (no es error).
- Requiere publicar el servicio (recurso `/SupervisorAdm`, operación `catsupervisorempleado..GetSupervisor`).

---
**Desarrollado por:** SIS10 - JASS
