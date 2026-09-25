# Servicio REST — Precio y Existencia de Artículo

---

## 1. Título del Servicio

**Art — Precio y Existencia de Artículo**
Consulta integral de un artículo: datos generales, proveedores, precios por presentación y
color con ofertas vigentes, colores, sustitutos, complementarios y existencias por almacén
(incluye mercancía en tránsito).
Programa servidor: `preciosartexist.p` · Procedure: `GetExistArt` · Modo: `single-run`

---

## 2. Método HTTP

```
GET
```

---

## 3. URL Completa

```
http://192.0.1.14:8828/RestADOSArt/rest/RestADOSArt/PrecioArticuloExist
```

---

## 4. Parámetros de Entrada (Query String)

> ✅ = Requerido · ❌ = Opcional

**`IdArticulo`** · STRING · ✅ — Código del artículo (`Articulo.Id-Articulo`, 6 caracteres)

**`IdUser`** · STRING · ✅ — Usuario que consulta (`Usuario.Id-User`)
- Su ubicación define los almacenes que suman existencia en `ttComplementarios`
- Ubicación `02B` = `02B` · `02R` · `FUG` · `02A` · `02C` · otra ubicación = solo ese almacén

**`IdProv`** · INTEGER · ❌ — Proveedor a consultar (último precio de proveedor y datos de compra)
- `0` u omitido = proveedor principal del artículo

---

## 5. Body

No aplica. Método `GET`.

---

## 6. Headers

| Header   | Valor              |
|----------|--------------------|
| `Accept` | `application/json` |

---

## 7. Ejemplo de Request

```
GET http://192.0.1.14:8828/RestADOSArt/rest/RestADOSArt/PrecioArticuloExist
    ?IdArticulo=169701
    &IdUser=olds
```

---

## 8. Ejemplo de Response

```json
{
  "response": {
    "IdError": false,
    "Respuesta": "OK",
    "dsExistArt": {
      "dsExistArt": {
        "ttArticulo": [
          {
            "IdArticulo": "169701",
            "Descr": "ROMPECABEZAS MADERA MELISSA&DOUG 6073071 CHUNKY SAFARI 8 PZA.",
            "Modelo": "6073071 CHUNKY SAFARI",
            "IdClavePS": "60141105",
            "IdTipoArt": "C",
            "IdProv": 2126,
            "Nombre": "SPIN MASTER MEXICO, S.A. DE C.V.",
            "Relevancia": 0,
            "Iniciales": "ABC",
            "Comprador": "NOMBRE DEL COMPRADOR",
            "DescrUDC": "PZA",
            "Equiv": 1.0,
            "DescrUMI": "PZA",
            "TotExist": 39.0,
            "Referencia": "",
            "IdFamilia": 424,
            "FamiliaDescr": "MATERIALES EDUCATIVOS,DIDACTICOS Y MANUALIDADES",
            "Procedencia": "",
            "IdGrupo": 1697,
            "GrupoDescr": "JUGUETES EDUCATIVOS",
            "IdMarca": 3924,
            "MarcaDescr": "MELISSA&DOUG",
            "Actualizacion": "2026-06-26",
            "IdCategoria": "DJ",
            "CategoDescr": "DIDACTICOS, JUEGOS Y JUGUETES",
            "FecReg": "2026-06-09",
            "MonedaNombre": "PESO",
            "CostoImporte": 1.0,
            "CargoExtra": 0.0,
            "PorcDesc": 0.0,
            "IncPtoPago": false
          }
        ],
        "ttArtUbic": [
          {
            "IdArticulo": "169701", "IdColor": 0, "IdTipoArt": "C", "ColorDescr": "",
            "IdAlm": "02B", "Minimo": 0.0, "Maximo": 0.0, "Exist": 8.0,
            "Transito": "", "Compro": 5.0, "CPM": 0.0, "VPM": 0.0
          },
          {
            "IdArticulo": "169701", "IdColor": 0, "IdTipoArt": "C", "ColorDescr": "",
            "IdAlm": "8", "Minimo": 0.0, "Maximo": 0.0, "Exist": 0.0,
            "Transito": "+T", "Compro": 0.0, "CPM": 0.0, "VPM": 0.0
          },
          {
            "IdArticulo": "169701", "IdColor": 0, "IdTipoArt": "C", "ColorDescr": "",
            "IdAlm": "9", "Minimo": 0.0, "Maximo": 0.0, "Exist": 3.0,
            "Transito": "+T", "Compro": 0.0, "CPM": 0.0, "VPM": 0.0
          }
        ],
        "ttArtProv": [
          { "IdArticulo": "169701", "IdProv": 2126, "NomProv": "SPIN MASTER MEXICO, S.A. DE C.V." }
        ],
        "ttArtPrecios": [
          {
            "IdArticulo": "169701", "IdPres": 3, "PresDescr": "*PZA", "ColorDescr": "",
            "PMayoreo": 191.81, "PMayoreoIVA": 222.5, "Tienda": 222.5, "Web": 222.5,
            "OfeContado": 0.0, "OfeCredito": 0.0
          },
          {
            "IdArticulo": "169701", "IdPres": 4, "PresDescr": "C/12", "ColorDescr": "",
            "PMayoreo": 2297.41, "PMayoreoIVA": 2665.0, "Tienda": 2665.0, "Web": 2665.0,
            "OfeContado": 0.0, "OfeCredito": 0.0
          }
        ]
      }
    }
  }
}
```

> Las tablas sin registros **no aparecen** en el JSON (en el ejemplo: `ttColores`,
> `ttSustitutos` y `ttComplementarios`). Fechas en formato `YYYY-MM-DD`.
> Todas las tablas traen `IdArticulo` para relacionarlas con `ttArticulo`.

### Campos de `ttArticulo` (1 registro)

**`IdArticulo`** · STRING — Código del artículo
**`Descr`** · STRING — Descripción
**`Modelo`** · STRING — Modelo
**`IdClavePS`** · STRING — Clave de producto / servicio SAT
**`IdTipoArt`** · STRING — Tipo de artículo
**`IdProv`** · INTEGER — Proveedor consultado
**`Nombre`** · STRING — Nombre del proveedor
**`Relevancia`** · INTEGER — Relevancia del artículo
**`Iniciales`** · STRING — Iniciales del comprador del proveedor
**`Comprador`** · STRING — Nombre del comprador
**`DescrUDC`** · STRING — Presentación de compra del proveedor
**`Equiv`** · DECIMAL — Equivalencia de la presentación de compra
**`DescrUMI`** · STRING — Presentación base en que se expresan las existencias
**`TotExist`** · DECIMAL — Existencia total de todas las ubicaciones (presentación base)
**`Referencia`** · STRING — Referencia del último precio de proveedor
**`IdFamilia`** · INTEGER — Familia
**`FamiliaDescr`** · STRING — Descripción de la familia
**`Procedencia`** · STRING — Procedencia
**`IdGrupo`** · INTEGER — Grupo
**`GrupoDescr`** · STRING — Descripción del grupo
**`IdMarca`** · INTEGER — Marca
**`MarcaDescr`** · STRING — Descripción de la marca
**`Actualizacion`** · DATE — Fecha del último precio de proveedor
**`IdCategoria`** · STRING — Categoría
**`CategoDescr`** · STRING — Descripción de la categoría
**`FecReg`** · DATE — Fecha de alta del artículo
**`MonedaNombre`** · STRING — Moneda del precio de proveedor
**`CostoImporte`** · DECIMAL — Importe del último precio de proveedor
**`CargoExtra`** · DECIMAL — Cargo extra del proveedor
**`PorcDesc`** · DECIMAL — % de descuento (del artículo o, si no tiene, del grupo)
**`IncPtoPago`** · BOOLEAN — `true` = incluye descuento por pronto pago

### Campos de `ttArtUbic` (existencias por almacén)

**`IdColor`** · INTEGER — Color
**`IdTipoArt`** · STRING — Tipo del color o del artículo; `"/"` = almacén con restricción de traspaso
**`ColorDescr`** · STRING — Descripción del color (10 caracteres)
**`IdAlm`** · STRING — Almacén
**`Minimo`** · DECIMAL — Mínimo (presentación base)
**`Maximo`** · DECIMAL — Máximo (presentación base)
**`Exist`** · DECIMAL — Existencia (presentación base)
**`Transito`** · STRING — `"+T"` = mercancía en tránsito hacia el almacén; vacío = sin tránsito
**`Compro`** · DECIMAL — Comprometido (presentación base)
**`CPM`** · DECIMAL — Siempre `0` en este servicio (ver `ArtUbicCPM`, sección 11)
**`VPM`** · DECIMAL — Siempre `0` en este servicio (ver `ArtUbicVPM`, sección 11)

### Campos de `ttArtProv` (proveedores del artículo)

**`IdProv`** · INTEGER — Proveedor
**`NomProv`** · STRING — Nombre del proveedor

### Campos de `ttArtPrecios` (precios por presentación y color)

**`IdPres`** · INTEGER — Presentación
**`PresDescr`** · STRING — Descripción; `*` al inicio = presentación de mayoreo
**`ColorDescr`** · STRING — Abreviatura del color con oferta o `Todos`
**`PMayoreo`** · DECIMAL — Precio de mayoreo (con oferta de crédito aplicada)
**`PMayoreoIVA`** · DECIMAL — Precio de mayoreo con IVA
**`Tienda`** · DECIMAL — Precio de tienda (el capturado en `Precios` si existe)
**`Web`** · DECIMAL — Precio web (el capturado en `Precios` si existe)
**`OfeContado`** · DECIMAL — % de oferta de contado
**`OfeCredito`** · DECIMAL — % de oferta de crédito

### Campos de `ttColores`

**`IdColor`** · INTEGER — Color
**`ColorDescr`** · STRING — Descripción
**`Abrev`** · STRING — Abreviatura
**`Tipo`** · STRING — Tipo de artículo del color
**`Activo`** · BOOLEAN — `true` = color activo
**`CveHex`** · STRING — Color en hexadecimal

### Campos de `ttSustitutos` (solo sustitutos activos)

**`IdArticulo`** · STRING — Código del **sustituto** (comportamiento heredado)
**`IdSust`** · STRING — Código del sustituto
**`Descr`** · STRING — Descripción del **artículo consultado** (comportamiento heredado)
**`PMay`** · DECIMAL — Precio de mayoreo del sustituto
**`Pres`** · STRING — Presentación de mayoreo del sustituto

### Campos de `ttComplementarios`

**`IdComp`** · STRING — Código del complementario
**`Descr`** · STRING — Descripción del complementario
**`PMay`** · DECIMAL — Precio de mayoreo
**`Exist`** · DECIMAL — Existencia en los almacenes de la ubicación del usuario
**`Pres`** · STRING — Presentación de mayoreo

---

## 9. Validaciones y Errores

Todas regresan HTTP 200 con `IdError: true` y el dataset vacío, salvo el caso 5.

**1. IdArticulo requerido** — `"ERROR: IdArticulo es obligatorio"`
**2. IdUser requerido** — `"ERROR: IdUser es obligatorio"`
**3. Usuario no existe** — `"ERROR: El usuario X no existe"`
**4. Artículo no existe** — `"ERROR: El articulo X no existe"`
**5. Sin costo** — `"No existen datos del costo del articulo"`
- Regresa `ttArticulo` y `ttArtProv` llenos; no calcula precios, colores, sustitutos,
  complementarios ni existencias

**6. Error inesperado** — `"ERROR: <mensaje ABL>"`

```json
{
  "response": {
    "IdError": true,
    "Respuesta": "ERROR: El articulo ZZZZZZ no existe",
    "dsExistArt": {
      "dsExistArt": {}
    }
  }
}
```

---

## 10. Reglas del Servicio

- **Presentación base:** existencias, mínimos, máximos y comprometido se expresan en la
  presentación activa tipo 3 de menor equivalencia (o en la UMI del artículo).
- **Ubicaciones:** solo almacenes existentes en el catálogo y colores activos.
- **`"+T"` por traspaso:** hay una solicitud de traspaso pendiente hacia el almacén.
- **`"+T"` por requisición:** fila adicional (existencia 0) para un almacén **sin** ubicación
  del artículo‑color que tiene una requisición pendiente.
- **`"/"` en `IdTipoArt`:** el almacén tiene restricción de traspaso para el artículo‑color.
- **Precios:** se aplican las ofertas vigentes (contado / crédito); si `Precios` tiene
  capturado precio de tienda o web, ese sustituye al calculado.

### Caché del tránsito

El cálculo del tránsito se guarda por artículo en la sesión del servidor durante una vigencia
configurable en la tabla `URL`:

**`Parametro`** · STRING — `PrecioArticuloExist.CacheMin`
**`Valor`** · STRING — Minutos de vigencia
- `0` = sin caché (siempre calcula) · sin registro o valor inválido = 5 minutos

La marca `+T` puede tardar hasta la vigencia en reflejar un cambio de estado de un traspaso o
requisición; existencias y precios siempre se leen al momento.

---

## 11. Otros Endpoints del Programa

Mismo programa `preciosartexist.p`; regresan solo `ttArtUbic` (mismos campos de la sección 8).

**`GET .../ArtUbicCPM?IdArt=`** · Procedure `GetCPM` — Existencias por almacén con `CPM`
(consumo promedio mensual de los últimos 180 días)

**`GET .../ArtUbicVPM?IdArt=`** · Procedure `GetVPM` — Existencias por almacén con `VPM`
(venta promedio mensual de los últimos 180 días)

---

**Desarrollado por:** SIS10 - JASS
