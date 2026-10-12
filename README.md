
# 🛒 Compra con Misti — Panel Admin

Panel de administración para gestionar la operación semanal de **Compra con Misti**.

Este repo contiene **solo el panel de administración**. El catálogo público y el resto del ecosistema viven en otro repo.

---

## ✨ Features

### 📊 Dashboard
- Pedidos activos, ingreso esperado, costo real y utilidad neta
- Top productos pedidos
- Comparación costo estimado vs costo real

### 🔀 Pipeline de pedidos
- Estados: `pendiente_pago → aceptado → empacado → entregado`
- Retroceso con motivo obligatorio (queda en historial)
- Filtro por nombre, celular y zona

### 🧮 Calculadora
- Creación manual de pedidos (para clientes por teléfono)
- Autocompletado de cliente por celular
- Detección automática de zona por distrito
- Mapa con lat/lng editable

### 📦 Productos
- Maestro de 24 productos con código `PR-XXX`
- Agrupados en Básico / Sazón / Especial
- Nunca se borran: solo se activan o desactivan

### 💰 Precios por ronda
- Edición de `precio_ref_mayorista` y `margen`
- Cálculo automático de `precio_venta = ref / (1 - margen)`
- Comparación con ronda anterior (Δ)
- Copiar precios de ronda anterior
- Publicación oficial de precios

### 🛒 Lista de compra
- Consolidado de todos los pedidos aceptados
- Registro de precio real pagado el domingo
- Cálculo de **margen real** por producto
- Comparación contra referencia mayorista

### 🗺️ Entregas
- División automática por zonas (A / B / C)
- Armado de viajes según capacidad del repartidor
- Optimización de rutas (algoritmo del vecino más cercano)
- Límite de 9 paradas por viaje (Google Maps)
- Generación de link de Google Maps + mensaje WhatsApp

### 🛵 Repartidores
- Gestión por zona, vehículo y capacidad de carga
- Modo de viaje automático (a pie, bici, moto, auto)

### 💸 Gastos / Excedentes / Reclamos
- Registro con tipo, monto y medio de pago
- Afectan el cálculo de utilidad neta

### 📈 Cierre y reportes
- Cálculo de utilidad neta por ronda
- Histórico de todas las rondas
- Export a PDF (imprimir)

### ⚙️ Configuración
- Capacidad de vehículos
- Máximo de paradas por viaje
- Datos de la oficina (dirección, lat, lng)
- Costos por defecto

### 🔄 Export / Import
- Export completo a **JSON**
- Export completo a **Excel** (multi-hoja)
- Import desde JSON o Excel
- Reset total de la base

---

## 🛠️ Stack

| Capa | Tecnología |
|---|---|
| **Frontend** | HTML + CSS + JavaScript (vanilla) |
| **Base de datos** | Firebase Firestore |
| **Auth** | Firebase Auth (Google) |
| **Mapas** | Leaflet + OpenStreetMap |
| **Geocoding** | Nominatim |
| **Excel** | SheetJS (XLSX) |
| **Fuente** | Google Fonts (Poppins) |

Sin frameworks. Sin build. Un solo archivo HTML autocontenido.

---

## 🚀 Setup

### 1. Requisitos

- Cuenta en [Firebase](https://firebase.google.com/)
- Un navegador moderno (Chrome, Edge, Firefox)

### 2. Configurar Firebase

1. Crear un proyecto en Firebase Console.
2. Activar **Firestore** (base de datos por defecto).
3. Activar **Authentication** → Google.
4. Copiar la config del proyecto (`apiKey`, `projectId`, etc.).

### 3. Editar el archivo

Abrí `index.html` y buscá:

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  // ...
};
```

Pegá tu propia config.

### 4. Cambiar el UID autorizado

Solo tu cuenta podrá entrar al panel. Buscá:

```js
const UID_AUTORIZADO = "s9FcWIZGZ5NjkN5934z1FCxpcsx2";
```

Reemplazá ese UID por el tuyo (lo sacás de Firebase Auth → Users después del primer login).

### 5. Abrir el panel

Abrí `index.html` en el navegador. Iniciá sesión con tu cuenta Google autorizada.

---

## 🗄️ Estructura de datos en Firestore

Las colecciones **se crean automáticamente** cuando el código escribe por primera vez. No hay que crearlas a mano.

```
rondas/
  R-YYYY-MM-DD/
    pedidos/
    items/
    precios/
    compras/
    entregas/
    gastos/
    excedentes/
    reclamos/

catalogo/
  productos/items/
  clientes/items/
  repartidores/items/

config/
  global/


## 🔄 Flujo completo del sistema

```
Cliente arma canasta en la web pública
        ↓
Envía el pedido por WhatsApp
        ↓
App Android (NotifBridge) captura la notificación
        ↓
POST al webhook de n8n
        ↓
n8n valida, parsea y escribe en Firestore
        ↓
Este panel lo ve en tiempo real
        ↓
Admin acepta, empaca, entrega
        ↓
Registra precio real → utilidad neta
```

> ⚠️ Este repo **solo contiene el panel admin**. El resto del sistema (catálogo público, n8n, app Android) vive en otros repos.


## 👤 Autor

**Dafne** · [@Daf1807](https://github.com/Daf1807)

Proyecto personal · Lima, Perú · 2026

---

## 📄 Licencia

Uso privado. Todos los derechos reservados.
