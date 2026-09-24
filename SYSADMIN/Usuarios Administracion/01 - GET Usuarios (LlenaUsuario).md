# 01 · GET /UsuariosAdm — LlenaUsuario

**Módulo:** SysAdmin / RestADOSASysAdmin
**Programa:** `RestADOSASysAdmin/AppServer/admusuarios.p`
**Procedimiento:** `LlenaUsuario` · **Verbo:** `GET`
**Autor:** SIS10 — JASS

---

## 1. Descripción

Consulta usuarios del sistema (tabla `Usuario`). Sin `IdUser` regresa **todos** los
usuarios; con `IdUser` regresa solo ese usuario. Por cada usuario resuelve y agrega
datos ligados de **Vendedor**, **Departamento** (vía `Empleado`), **Rol**, **Módulo**
y **Clase de Cliente**. Lectura con `NO-LOCK` (no bloquea la tabla).

Es el mismo endpoint que usa la pantalla `GestionUsuariosAdministracion.razor` para
cargar el listado y la ficha del usuario.

---

## 2. Endpoint

```
GET http://192.0.1.14:8823/RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm
```

---

## 3. Parámetros de entrada (Query String)

> ✅ = Requerido · ❌ = Opcional

**`IdUser`** · STRING · ❌ — Iniciales/Id del usuario a consultar
- Vacío o `?` = regresa **todos** los usuarios (modo catálogo)
- Con valor = regresa solo el usuario con ese `Id-User` (ej. `gee`)

---

## 4. Campos del response (ttUsuario)

**`IdUser`** · CHARACTER — Id/iniciales del usuario (`Usuario.Id-User`)
**`NomUsuario`** · CHARACTER — Nombre del usuario
**`IdCaja`** · CHARACTER — Id de caja
**`IdUbicacion`** · CHARACTER — Ubicación asignada
**`TipoUsuario`** · CHARACTER — Tipo de usuario
**`hisPassword`** · CHARACTER — Password histórico
**`IdFuncion`** · CHARACTER — Función (`id_funcion`)
**`Sistemas`** · LOGICAL — `true` = usuario de Sistemas
**`Facultado`** · LOGICAL — `true` = facultado
**`Foraneo`** · LOGICAL — `true` = foráneo
**`Clave`** · INTEGER — Clave numérica del usuario
**`Nivel`** · INTEGER — Nivel del usuario
**`Firma`** · CHARACTER — Firma
**`Depto`** · CHARACTER — Id de departamento (de `Empleado` ligado, o `Usuario.depto`)
**`DeptoDes`** · CHARACTER — Descripción del departamento
**`Telefono`** · CHARACTER — Teléfono
**`NumMenu`** · INTEGER — Número de menú
**`IdSup`** · INTEGER — Id del supervisor
**`IdCajero`** · INTEGER — Id de cajero
**`IdUbivta`** · CHARACTER — Ubicación de venta
**`Email`** · CHARACTER — Correo (`e-mail`)
**`Email2`** · CHARACTER — Correo secundario (`e-mail2`)
**`IdSeguro`** · CHARACTER — Id de seguro
**`IdVendedor`** · CHARACTER — Vendedor ligado (solo activos). Si `Id-User > "99999"` liga por `Vendedor.iniciales`; si no, usa el vendedor `0104`
**`FUG`** · LOGICAL — `true` por defecto; `false` cuando `Id-Ubicacion = "03A"`
**`IdModulo`** · INTEGER — Módulo (1–9)
**`ModuloDes`** · CHARACTER — Descripción del módulo (tabla `Modulo`)
**`IdRol`** · INTEGER — Rol del usuario
**`RolDes`** · CHARACTER — Descripción del rol (tabla `Roles`)
**`IdClaseCte`** · INTEGER — Clase de cliente
**`ClaseCteDes`** · CHARACTER — Descripción de la clase (tabla `ClaseCte`)
**`FecReg`** · DATE — Fecha de registro/alta

### Mapa de módulos (IdModulo)

`1` Sistemas · `2` Créditos · `3` Tesorería · `4` Sucursales · `5` Ventas
`6` Cuentas por Pagar · `7` Artículos · `8` Mercadotecnia · `9` Contabilidad

---

## 5. Ejemplos de uso

Catálogo completo (todos los usuarios):

```
GET /RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm
```

Un usuario específico:

```
GET /RestADOSASysAdmin/rest/RestADOSASysAdmin/UsuariosAdm?IdUser=gee
```

Respuesta (ejemplo, con envoltorio del adaptador clásico):

```json
{
  "response": {
    "ttUsuario": {
      "ttUsuario": [
        {
          "IdUser": "gee",
          "NomUsuario": "GERARDO EJEMPLO",
          "IdModulo": 1,
          "ModuloDes": "Sistemas",
          "IdRol": 2,
          "RolDes": "Director",
          "Nivel": 9,
          "Email": "gee@adosa.com.mx"
        }
      ]
    }
  }
}
```

---

## 6. Notas técnicas

- **Solo lectura** — `NO-LOCK`, no bloquea la tabla `Usuario`.
- El `IdUser` vacío trae la tabla completa de usuarios; filtra en el front si solo
  necesitas un subconjunto.
- Los campos `*Des` (ModuloDes, RolDes, ClaseCteDes, DeptoDes) y `IdVendedor` se
  resuelven en tiempo de consulta con `FIND` a sus catálogos; no viven en `Usuario`.

---

**Desarrollado por:** SIS10 - JASS
