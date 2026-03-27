# CatálogoGrupoEspecial — REST API

**Servidor:** `http://192.0.1.14:8861`
**Base URL:** `/RestADOSArt/rest/RestADOSArt/CatalogoGrupoEspecial`
**Programa:** `catalogo-GRUPO-ESPECIAL.p`
**Modo:** `single-run`

---

## Modelo de datos

El servicio maneja un **dataset jerárquico** (`dsSelGrupo`) con dos niveles:

- **Cabecera** (`ttSelGrupo`) — La selección de grupos (ej. "PRODUCTOS DE LIMPIEZA")
- **Detalle** (`ttDetSelGrupo`) — Los grupos que pertenecen a esa selección

```
ttSelGrupo  (1)
    └── ttDetSelGrupo  (N)
```

---

## Endpoints

| # | Método | Endpoint                                      | Procedure             |
|---|--------|-----------------------------------------------|-----------------------|
| 1 | GET    | `/CatalogoGrupoEspecial`                      | `GetSelecciones`      |
| 2 | POST   | `/CatalogoGrupoEspecial/CrearSeleccion`       | `CrearSeleccion`      |
| 3 | PUT    | `/CatalogoGrupoEspecial/ActualizarSeleccion`  | `ActualizarSeleccion` |
| 4 | DELETE | `/CatalogoGrupoEspecial/QuitarGrupo`          | `QuitarGrupo`         |

---

## 1. GET — Consultar Selecciones

**URLs de ejemplo:**
```
GET .../CatalogoGrupoEspecial
GET .../CatalogoGrupoEspecial?IdSelGrupo=A000072&IncluirDetalle=true
```

### Parámetros de entrada (Query String)

> ✅ = Requerido · ❌ = Opcional

**`IdSelGrupo`** · STRING · ❌ — Filtra por selección específica
- Vacío o ausente = devuelve todas las selecciones

**`IncluirDetalle`** · BOOLEAN · ❌ — Incluye los grupos del detalle en la respuesta
- `true` = incluye `ttDetSelGrupo` · `false` (default) = solo cabeceras

### Respuesta exitosa — sin detalle

```json
{
  "IdError": false,
  "Respuesta": " 3 seleccion(es) encontrada(s)",
  "dsSelGrupo": {
    "ttSelGrupo": [
      {
        "IdSelGrupo": "A000072",
        "Descr": "PRODUCTOS DE LIMPIEZA Y DESINFECCION",
        "TotalGrupos": 4
      }
    ],
    "ttDetSelGrupo": []
  }
}
```

### Respuesta exitosa — con detalle (`IncluirDetalle=true`)

```json
{
  "IdError": false,
  "Respuesta": " 1 seleccion(es) encontrada(s)",
  "dsSelGrupo": {
    "ttSelGrupo": [
      {
        "IdSelGrupo": "A000072",
        "Descr": "PRODUCTOS DE LIMPIEZA Y DESINFECCION",
        "TotalGrupos": 3
      }
    ],
    "ttDetSelGrupo": [
      { "IdSelGrupo": "A000072", "IdGrupo": 10, "DescrGrupo": "LIMPIEZA HOGAR"  },
      { "IdSelGrupo": "A000072", "IdGrupo": 15, "DescrGrupo": "DESINFECTANTES"  },
      { "IdSelGrupo": "A000072", "IdGrupo": 25, "DescrGrupo": "HIGIENE PERSONAL" }
    ]
  }
}
```

### Campos de respuesta (ttSelGrupo)

**`IdSelGrupo`** · STRING — Clave de la selección (ej. `A000072`)
**`Descr`** · STRING — Descripción de la selección
**`TotalGrupos`** · INTEGER — Número de grupos que tiene asignados

### Campos de respuesta (ttDetSelGrupo)

**`IdSelGrupo`** · STRING — Clave de la selección a la que pertenece
**`IdGrupo`** · INTEGER — Id del grupo
**`DescrGrupo`** · STRING — Descripción del grupo (calculado, no se almacena)

---

## 2. POST — Crear Selección

**URL de ejemplo:**
```
POST .../CatalogoGrupoEspecial/CrearSeleccion
```

> No lleva parámetros en Query String. Todo va en el body.

### Body (JSON)

```json
{
  "dsSelGrupo": {
    "ttSelGrupo": [
      {
        "IdSelGrupo": "",
        "Descr": "PRODUCTOS DE LIMPIEZA"
      }
    ],
    "ttDetSelGrupo": [
      { "IdSelGrupo": "", "IdGrupo": 10 },
      { "IdSelGrupo": "", "IdGrupo": 15 },
      { "IdSelGrupo": "", "IdGrupo": 20 }
    ]
  }
}
```

### Campos del body

**Cabecera `ttSelGrupo`:**

**`IdSelGrupo`** · STRING — Enviar vacío `""`. El folio se genera automáticamente desde tabla `Folio` con formato `Prefijo + número de 6 dígitos` (ej. `A000139`)
**`Descr`** · STRING — Descripción de la selección. **Obligatorio**

**Detalle `ttDetSelGrupo`:**

**`IdSelGrupo`** · STRING — Enviar vacío `""`. El sistema lo reemplaza con el folio generado
**`IdGrupo`** · INTEGER — Id del grupo a incluir. Debe existir en catálogo de Grupos

### Validaciones

- `Descr` es obligatorio
- Debe enviarse al menos un grupo en `ttDetSelGrupo`
- Todos los `IdGrupo` deben existir en la tabla `Grupo`
- Grupos duplicados en el mismo body se omiten silenciosamente

### Respuesta exitosa

```json
{
  "IdError": false,
  "Respuesta": " Seleccion A000139 creada con 3 grupo(s)"
}
```

### Errores posibles

```json
{ "IdError": true, "Respuesta": "ERROR: No se recibieron datos de la seleccion" }
{ "IdError": true, "Respuesta": "ERROR: La descripcion de la seleccion es requerida" }
{ "IdError": true, "Respuesta": "ERROR: Debe incluir al menos un grupo en la seleccion" }
{ "IdError": true, "Respuesta": "ERROR: Grupo 99 no existe" }
{ "IdError": true, "Respuesta": "ERROR: No se pudo generar folio. Tabla Folio no disponible" }
```

---

## 3. PUT — Actualizar Selección

**URL de ejemplo:**
```
PUT .../CatalogoGrupoEspecial/ActualizarSeleccion
```

> No lleva parámetros en Query String. Todo va en el body.

### Body (JSON)

```json
{
  "dsSelGrupo": {
    "ttSelGrupo": [
      {
        "IdSelGrupo": "A000139",
        "Descr": "PRODUCTOS DE LIMPIEZA Y DESINFECCION"
      }
    ],
    "ttDetSelGrupo": [
      { "IdSelGrupo": "A000139", "IdGrupo": 10 },
      { "IdSelGrupo": "A000139", "IdGrupo": 15 },
      { "IdSelGrupo": "A000139", "IdGrupo": 25 }
    ]
  }
}
```

### Comportamiento del detalle

> ⚠️ **REEMPLAZO COMPLETO** — El servicio **elimina todos los grupos existentes** de la selección y los recrea con los del body. No es una actualización incremental.

Si la selección tenía grupos `[10, 15, 20]` y se envía `[10, 15, 25]`:
- Se eliminan: `10, 15, 20`
- Se crean: `10, 15, 25`

### Campos del body

**Cabecera `ttSelGrupo`:**

**`IdSelGrupo`** · STRING — Id de la selección a modificar. **Obligatorio**
**`Descr`** · STRING — Nueva descripción. **Obligatorio**

**Detalle `ttDetSelGrupo`:**

**`IdSelGrupo`** · STRING — Debe coincidir con el `IdSelGrupo` de la cabecera
**`IdGrupo`** · INTEGER — Id del grupo. Debe existir en catálogo de Grupos

### Validaciones

- `IdSelGrupo` y `Descr` son obligatorios
- La selección debe existir en BD
- Todos los `IdGrupo` deben existir en la tabla `Grupo`
- Grupos duplicados se omiten

### Respuesta exitosa

```json
{
  "IdError": false,
  "Respuesta": " Seleccion A000139 actualizada con 3 grupo(s)"
}
```

### Errores posibles

```json
{ "IdError": true, "Respuesta": "ERROR: No se recibieron datos de la seleccion" }
{ "IdError": true, "Respuesta": "ERROR: IdSelGrupo es requerido" }
{ "IdError": true, "Respuesta": "ERROR: Seleccion bloqueada por otro usuario" }
{ "IdError": true, "Respuesta": "ERROR: Seleccion A000139 no existe" }
{ "IdError": true, "Respuesta": "ERROR: La descripcion es requerida" }
{ "IdError": true, "Respuesta": "ERROR: Grupo 99 no existe" }
```

---

## 4. DELETE — Quitar Grupo de Selección

**URL de ejemplo:**
```
DELETE .../CatalogoGrupoEspecial/QuitarGrupo?IdSelGrupo=A000139&IdGrupo=25
```

### Parámetros de entrada (Query String)

> ✅ = Requerido

**`IdSelGrupo`** · STRING · ✅ — Id de la selección
**`IdGrupo`** · INTEGER · ✅ — Id del grupo a quitar

> Quita **únicamente ese grupo** del detalle. La cabecera y los demás grupos no se modifican.

### Respuesta exitosa

```json
{
  "IdError": false,
  "Respuesta": " Grupo 25 quitado de seleccion A000139"
}
```

### Errores posibles

```json
{ "IdError": true, "Respuesta": "ERROR: IdSelGrupo es requerido" }
{ "IdError": true, "Respuesta": "ERROR: IdGrupo es requerido" }
{ "IdError": true, "Respuesta": "ERROR: Registro bloqueado por otro usuario" }
{ "IdError": true, "Respuesta": "ERROR: Grupo 25 no existe en la seleccion A000139" }
```

---

## Estructura de respuesta general

Todos los endpoints devuelven al menos:

**`IdError`** · BOOLEAN — `false` = exitoso · `true` = error
**`Respuesta`** · STRING — Mensaje descriptivo del resultado o del error
