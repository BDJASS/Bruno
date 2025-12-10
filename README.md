# Colecciones Bruno - Equipo ADOSA

Este repositorio contiene todas las **colecciones de Bruno** utilizadas por el equipo ADOSA.  
Actualmente se comparten únicamente los módulos necesarios para el equipo:

- **Artículos**  
- **Cuentas por Pagar (CxP)**  
- **Tesorería**
- **Ventas**

> Otros módulos no están disponibles en este repo para mantener el control, pero se integraran si son necesarios.

---

## 📂 Estructura del repositorio

Bruno/
- articulos/ # Colecciones de artículos
- cxp/ # Colecciones de cuentas por pagar
- tesoreria/ # Colecciones de tesorería


Cada carpeta contiene los requests `.bru` y subcarpetas necesarias para Bruno.

---

## 🟢 Para el equipo ADOSA

### 1️⃣ Primera vez que usan el repo

1. Abrir **PowerShell** (o terminal de su preferencia).  
2. Navegar a la carpeta donde quieran guardar las colecciones.  
3. Ejecutar:

```bash
git clone https://github.com/BDJASS/Bruno.git
Esto descargará las colecciones permitidas.
```

<img width="1117" height="185" alt="image" src="https://github.com/user-attachments/assets/b8b3d707-62a8-4f94-b98c-3c8bfd658e95" />


### 2️⃣ Abrir las colecciones en Bruno

Abrir Bruno en su computadora.

Ir a File > Open Collection.

<img width="415" height="340" alt="image" src="https://github.com/user-attachments/assets/4910c0e5-e7f7-462a-903b-7cf0bf05449b" />

Seleccionar la colección dentro de la carpeta correspondiente del repo.


### 3️⃣ Descargar actualizaciones futuras

Cada vez que se agreguen nuevas colecciones o se actualicen:
```bash
cd Bruno
git pull

```

⚠️ Importante para el equipo

No modificar ni subir cambios al repositorio.

Solo descargar actualizaciones con git pull.

Mantener la estructura de carpetas para que Bruno pueda localizar los requests correctamente.
