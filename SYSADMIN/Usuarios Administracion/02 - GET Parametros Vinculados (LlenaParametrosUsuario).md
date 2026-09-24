# 02 · GET /UsuariosAdm/Parametros — LlenaParametrosUsuario

**Módulo:** SysAdmin / RestADOSASysAdmin
**Programa:** `RestADOSASysAdmin/AppServer/admusuarios.p`
**Procedimiento:** `LlenaParametrosUsuario` · **Verbo:** `GET`
**Autor:** SIS10 — JASS

---

## 1. Descripción

Regresa las parametrizaciones (tabla `URL`) a las que está **ligado** un usuario.
Alimenta la pestaña **"Parámetros Vinculados"** de la ficha de usuario, para saber a
qué parámetros pertenece un usuario (por si se quiere replicar esa misma
configuración en otro usuario). **Solo lectura** — el catálogo `URL` se edita desde
el módulo de Catálogos, no desde aquí.

---

## 2. Endpoint

```
GET http://192.0.1.14:8823/RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm/Parametros
```

---

## 3. Parámetros de entrada (Query String)

> ✅ = Requerido · ❌ = Opcional

**`IdUser`** · STRING · ✅ — Iniciales/Id del usuario (ej. `gee`)
- Vacío o `?` = **no regresa nada** (temp-table vacía); no es un error

---

## 4. Cómo hace el match

`URL.Valor` **no** es un valor único: es una lista separada por comas
(ej. `"gee,elf,franc"`). El match se hace con `CAN-DO(URL.Valor, IdUser)`, es decir,
el usuario debe ser **uno de los elementos** de esa lista, no una igualdad exacta del
campo completo.

---

## 5. Campos del response (ttParametrosUsuario)

**`IdURL`** · INTEGER — Id de la parametrización (`URL.Id-URL`)
**`Parametro`** · CHARACTER — Nombre del parámetro (`URL.Parametro`)
**`Descr`** · CHARACTER — Descripción del parámetro
**`Valor`** · CHARACTER — Lista completa de usuarios ligados (CSV, ej. `"gee,elf,franc"`)

---

## 6. Ejemplo de uso

```
GET /RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm/Parametros?IdUser=gee
```

Respuesta (ejemplo, con envoltorio del adaptador clásico):

```json
{
  "response": {
    "ttParametrosUsuario": {
      "ttParametrosUsuario": [
        {
          "IdURL": 12,
          "Parametro": "CORREO_COPIA_PEDIDOS",
          "Descr": "Usuarios que reciben copia de pedidos",
          "Valor": "gee,elf,franc"
        }
      ]
    }
  }
}
```

---

## 7. Notas técnicas

- **Solo lectura** — no modifica la tabla `URL`.
- Regresa el campo `Valor` completo (CSV), útil para ver junto a qué otros usuarios
  está agrupado el consultado.

---

**Desarrollado por:** SIS10 - JASS
