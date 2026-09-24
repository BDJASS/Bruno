# 05 · DELETE /UsuariosAdm — DeleteUsuarios

**Módulo:** SysAdmin / RestADOSASysAdmin
**Programa:** `RestADOSASysAdmin/AppServer/admusuarios.p`
**Procedimiento:** `DeleteUsuarios` · **Verbo:** `DELETE`
**Autor:** SIS10 — JASS

---

## 1. Descripción

**Elimina** un usuario de la tabla `Usuario` por su `IdUser`. Localiza el registro con
`EXCLUSIVE-LOCK` dentro de `DO TRANSACTION` y, si existe (`AVAILABLE`), lo borra.

---

## 2. Endpoint

```
DELETE http://192.0.1.14:8823/RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm?IdUser=PRUEBA01
```

---

## 3. Parámetros de entrada (Query String)

> ✅ = Requerido · ❌ = Opcional

**`IdUser`** · STRING · ✅ — Id/iniciales del usuario a eliminar (ej. `PRUEBA01`)

---

## 4. Campos de response

Sin parámetros de salida (el procedimiento no define `OUTPUT`). El resultado se
refleja en el código HTTP de la respuesta.

---

## 5. Ejemplo de uso

```
DELETE /RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm?IdUser=PRUEBA01
```

---

## 6. Notas técnicas

- ⚠️ **Operación destructiva e irreversible.** Si el `IdUser` existe, el registro se
  borra sin confirmación adicional. Apunta siempre a un usuario de prueba (`PRUEBA01`).
- Si el `IdUser` no existe, no hay error: simplemente no borra nada.
- Borra solo el registro de `Usuario`; **no** hace cascada sobre datos relacionados
  (vendedor, permisos, empleado, etc.).

---

**Desarrollado por:** SIS10 - JASS
