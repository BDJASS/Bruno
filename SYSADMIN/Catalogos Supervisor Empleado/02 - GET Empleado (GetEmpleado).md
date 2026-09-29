# 02 · GET /EmpleadoAdm — GetEmpleado

**Programa:** `RestADOSASysAdmin/AppServer/catsupervisorempleado.p`
**Procedimiento:** `GetEmpleado` · **Verbo:** `GET` · **Autor:** SIS10 — JASS

## 1. Descripción

Catálogo de **empleados** (tabla `Empleado`). Lo consume `GestionUsuariosAdministracion.razor` (campo Nombre: búsqueda por nombre o número de empleado). Regresa activos **e inactivos**; el campo `Activo` permite al front filtrarlos. Lectura con `NO-LOCK`; ordenado por `Nombre`.

## 2. Endpoint

`GET http://192.0.1.14:8813/RestADOSASysAdmin/rest/RestADOSASysAdmin/EmpleadoAdm`

## 3. Parámetros de entrada (Query String)

Ninguno.

## 4. Parámetros de salida

**`IdError`** · LOGICAL — `false` = consulta exitosa; `true` = ocurrió un error
**`Respuesta`** · CHARACTER — `"OK"` en éxito; `"Error: <mensaje>"` si falló (capturado por `CATCH`)
**`ttEmpleado`** · TABLE — Un registro por empleado

## 5. Campos del response (ttEmpleado)

**`IdNomina`** · CHARACTER — Número de empleado (`Empleado.Id-Nomina`)
**`Nombre`** · CHARACTER — Nombre del empleado
**`Iniciales`** · CHARACTER — Iniciales (`Empleado.Iniciales` = `Usuario.Id-User`)
**`Activo`** · LOGICAL — `true` = empleado activo; `false` = dado de baja

## 6. Ejemplos de uso

Éxito (envoltorio del adaptador clásico):

```json
{
  "response": {
    "IdError": false,
    "Respuesta": "OK",
    "ttEmpleado": {
      "ttEmpleado": [
        { "IdNomina": "001234", "Nombre": "DELGADO SANCHEZ OSCAR LESTER", "Iniciales": "olds", "Activo": true }
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

- **Solo lectura** — `NO-LOCK`, no bloquea la tabla `Empleado`.
- No filtra por `Activo`; regresa la tabla completa (puede ser volumen alto, filtra en el front).
- Requiere publicar el servicio (recurso `/EmpleadoAdm`, operación `catsupervisorempleado..GetEmpleado`).

---
**Desarrollado por:** SIS10 - JASS
