# 04 · PUT /UsuariosAdm — PutUsuarios

**Módulo:** SysAdmin / RestADOSASysAdmin
**Programa:** `RestADOSASysAdmin/AppServer/admusuarios.p`
**Procedimiento:** `PutUsuarios` · **Verbo:** `PUT`
**Autor:** SIS10 — JASS

---

## 1. Descripción

**Actualiza** un usuario existente. Toma la primera fila de `ttUsuario`
(`FIND FIRST ttUsuario`), localiza el `Usuario` cuyo `Id-User = ttUsuario.IdUser` con
`EXCLUSIVE-LOCK` y sobrescribe sus campos. A diferencia del alta, **no** modifica
`FecReg` (conserva la fecha original).

---

## 2. Endpoint

```
PUT http://192.0.1.14:8823/RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm
Content-Type: application/json
```

---

## 3. Entrada (body JSON — ttUsuario)

> ✅ = Requerido · ❌ = Opcional · Se envía **una** fila en `ttUsuario`.

**`IdUser`** · CHARACTER · ✅ — Llave del match; identifica el usuario a actualizar
- Resto de campos: los mismos que en el alta (endpoint 03). Se reasignan tal cual llegan.

> Si el `IdUser` no existe, el `FIND ... EXCLUSIVE-LOCK` no encuentra registro y no se
> actualiza nada. `FecReg` no se toca.

---

## 4. Campos de response

**`Mensaje`** · CHARACTER — Mensaje de resultado devuelto por el servicio.

---

## 5. Ejemplo de uso

Body (reutiliza `PRUEBA01` del endpoint 03 y cambia Nombre, Facultado, Nivel y Teléfono):

```json
{
  "ttUsuario": [
    {
      "IdUser": "PRUEBA01",
      "NomUsuario": "USUARIO DE PRUEBA (EDITADO)",
      "IdUbicacion": "01A",
      "TipoUsuario": "N",
      "hisPassword": "1234",
      "Facultado": true,
      "Nivel": 2,
      "Telefono": "5544332211",
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

- `BUFFER-COPY ttUsuario TO Usuario` seguido de `ASSIGN` explícito de cada campo.
- El servicio no valida existencia previa: si el `IdUser` no está, simplemente no
  hay actualización (revísalo del lado del consumidor si necesitas confirmación).

---

**Desarrollado por:** SIS10 - JASS
