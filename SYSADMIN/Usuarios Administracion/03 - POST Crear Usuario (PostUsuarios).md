# 03 · POST /UsuariosAdm — PostUsuarios

**Módulo:** SysAdmin / RestADOSASysAdmin
**Programa:** `RestADOSASysAdmin/AppServer/admusuarios.p`
**Procedimiento:** `PostUsuarios` · **Verbo:** `POST`
**Autor:** SIS10 — JASS

---

## 1. Descripción

Da de **alta** un usuario nuevo en la tabla `Usuario` a partir de la fila enviada en
`ttUsuario`. La fecha de registro (`FecReg`) se asigna automáticamente a `TODAY` (no
se toma del body). Corre dentro de `DO TRANSACTION`.

---

## 2. Endpoint

```
POST http://192.0.1.14:8823/RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm
Content-Type: application/json
```

---

## 3. Entrada (body JSON — ttUsuario)

> ✅ = Requerido · ❌ = Opcional · Se envía **una** fila en `ttUsuario`.

**`IdUser`** · CHARACTER · ✅ — Id/iniciales del usuario (clave)
**`NomUsuario`** · CHARACTER · ❌ — Nombre
**`IdCaja`** · CHARACTER · ❌ — Id de caja
**`IdUbicacion`** · CHARACTER · ❌ — Ubicación
**`TipoUsuario`** · CHARACTER · ❌ — Tipo de usuario
**`hisPassword`** · CHARACTER · ❌ — Password
**`IdFuncion`** · CHARACTER · ❌ — Función (`id_funcion`)
**`Sistemas`** · LOGICAL · ❌ — `true`/`false`
**`Facultado`** · LOGICAL · ❌ — `true`/`false`
**`Foraneo`** · LOGICAL · ❌ — `true`/`false`
**`Clave`** · INTEGER · ❌ — Clave numérica
**`Nivel`** · INTEGER · ❌ — Nivel
**`Firma`** · CHARACTER · ❌ — Firma
**`Telefono`** · CHARACTER · ❌ — Teléfono
**`NumMenu`** · INTEGER · ❌ — Número de menú
**`IdSup`** · INTEGER · ❌ — Id de supervisor
**`IdCajero`** · INTEGER · ❌ — Id de cajero
**`IdUbivta`** · CHARACTER · ❌ — Ubicación de venta
**`Email`** · CHARACTER · ❌ — Correo
**`Email2`** · CHARACTER · ❌ — Correo secundario
**`IdSeguro`** · CHARACTER · ❌ — Id de seguro
**`IdModulo`** · INTEGER · ❌ — Módulo (1–9)
**`IdRol`** · INTEGER · ❌ — Rol
**`IdClaseCte`** · INTEGER · ❌ — Clase de cliente

> Campos de solo-display (`ModuloDes`, `RolDes`, `ClaseCteDes`, `DeptoDes`,
> `IdVendedor`, `FUG`) no se persisten en el alta; se resuelven al leer (endpoint 01).
> `FecReg` se fija a `TODAY` internamente.

---

## 4. Campos de response

**`Mensaje`** · CHARACTER — Mensaje de resultado devuelto por el servicio.

---

## 5. Ejemplo de uso

Body:

```json
{
  "ttUsuario": [
    {
      "IdUser": "PRUEBA01",
      "NomUsuario": "USUARIO DE PRUEBA",
      "IdUbicacion": "01A",
      "TipoUsuario": "N",
      "hisPassword": "1234",
      "Nivel": 1,
      "Email": "prueba01@adosa.com.mx",
      "IdModulo": 1,
      "IdRol": 0,
      "IdClaseCte": 0
    }
  ]
}
```

> Si el servidor rechaza el body plano, usa la forma anidada del adaptador clásico:
> `{ "ttUsuario": { "ttUsuario": [ { ... } ] } }`.

---

## 6. Notas técnicas

- No valida duplicados de `IdUser` de forma explícita: enviar un `IdUser` ya
  existente puede fallar por el índice único `idx-user-pas`. Usa un Id nuevo (ej.
  `PRUEBA01`) para probar el alta.
- `BUFFER-COPY ttUsuario TO Usuario` seguido de `ASSIGN` explícito de cada campo.

---

**Desarrollado por:** SIS10 - JASS
