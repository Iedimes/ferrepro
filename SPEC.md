# Sistema Integral para Ferretería "FerrePro"

## 1. Overview del Proyecto

**Nombre:** FerrePro - Sistema Integral de Gestión para Ferretería  
**Tipo:** Sistema Web con API REST y App Móvil  
**Usuarios:** Dueños, vendedores, administradores de una ferretería de barrio en crecimiento  
**Funcionalidad Principal:** Gestión completa de inventario, ventas, facturación, cuentas por cobrar y cobros con control de deudas

---

## 2. Características del Sistema

### 2.1 Módulos del Sistema

#### A) Gestión de Inventario
- **Productos:**
  - Código interno, código de barras, nombre, descripción
  - Categorías (herramientas, pinturas, tornillos, eléctrico, fontanería, etc.)
  - Proveedor asociado
  - Unidad de medida (unidad, kilo, metro, litro)
  - Precio costo, precio venta, precio mayorista
  - Stock mínimo (alerta), stock actual
  - Producto activo/inactivo

- **Movimientos de Stock:**
  - Entradas (compras a proveedores)
  - Salidas (ventas, devoluciones, mermas)
  - Historial de movimientos por producto
  - Ajuste de inventario

- **Alertas:**
  - Stock bajo mínimo
  - Productos sin movimiento (x días)
  - Precios de costo mayores a precio venta

#### B) Clientes y Proveedores
- **Clientes:**
  - Datos personales/comercio
  - Teléfono, dirección, email
  - Límite de crédito (monto máximo)
  - Categoría (minorista, mayorista, revendedor)
  - Deuda actual visible

- **Proveedores:**
  - Datos comerciales
  - Productos que surte
  - Condiciones de pago

#### C) Punto de Venta (POS)
- **Funcionalidades:**
  - Búsqueda por código, nombre o código de barras
  - Carrito de ventas
  - Descuentos por producto o total (%)
  - Métodos de pago: Efectivo, Transferencia, Tarjeta, Mixto
  - Forma de entrega: Mostrador, Delivery, Pendiente (se anota para cobrar después)
  - Impresión de ticket/pdf
  - Venta rápida

- **Control Especial:**
  - "Anotar" = dejar pendiente sin cobrar (para cobrar después)
  - Si hay deuda anterior, mostrar alerta
  - Preventa: productos pedidos que luego se facturan al pagar

#### D) Facturación
- **Tipos de comprobante:**
  - Ticket (no fiscal)
  - Factura A/B/C (fiscal - configurable por país)
  - Nota de crédito
  - Nota de débito

- **Numeración:**
  - Serie y número automático
  - Control de CAE (Argentina) o similar

#### E) Cuentas por Cobrar (CxC)
- **Control de Deudas:**
  - Cada cliente puede tener deuda
  - Historial de facturas pendientes
  - Estados: Pendiente, Parcial, Cancelado, Vencido

- **Cobros:**
  - Registrar pagos parciales o totales
  - Diferentes métodos de pago
  - Aplicar pagos a facturas específicas
  - Intereses por mora (configurable)

- **Preventas/Pedidos:**
  - Cuando un cliente "anota" se crea un registro de "cuenta pendiente"
  - Al ir a pagar, se genera la factura
  - Opcional: pagar sin factura (solo registro de cobro)

#### F) Cuentas por Pagar (CxP)
- **Gestión de deudas a proveedores**
- Registro de compras a crédito
- Programación de pagos
- Estados: Pendiente, Parcial, Cancelado

#### G) Reportes
- **Ventas:**
  - Por día, semana, mes
  - Por vendedor
  - Por categoría/producto
  - Mejores vendedores
  - Mejores productos

- **Stock:**
  - Inventario actual
  - Productos con stock bajo
  - Rotación de inventario

- **Financieros:**
  - Estado de resultados
  - Deudas de clientes
  - Deudas a proveedores
  - Ganancias por producto

#### H) Configuración
- **Empresa:**
  - Nombre, logo, dirección, CUIT/tax ID
  - Configuración fiscal (tipo de factura)

- **Usuarios y Permisos:**
  - Roles: Admin, Vendedor, Supervisor
  - Permisos por módulo

- **Parámetros:**
  - Porcentaje de ganancia por defecto
  - Alertas de stock
  - Intereses por mora

---

## 3. Stack Tecnológico

### Backend
- **Tecnología:** PHP 8.0 con SQLite (embebido)
- **Sin dependencias externas** (sin Composer)
- **Estructura:** PHP nativo con patrón MVC simple
- **Autenticación:** Sesiones PHP

### Frontend Web
- **HTML5** con Bootstrap 5
- **JavaScript** vanilla para interactividad
- **CSS:** Bootstrap Icons

### Deployment
- **Servidor:** Cualquier servidor PHP (WAMP/XAMPP)

---

## 4. Estructura de Base de Datos (Tablas Principales)

```
users
categories
products
providers
clients
sales (cabecera de venta)
sale_details (ítems)
sale_payments (métodos de pago)
invoices (facturas)
invoice_details
accounts_receivable (cuentas por cobrar)
account_receivable_payments (cobros)
accounts_payable (cuentas por pagar)
stock_movements
settings
```

---

## 5. Flujos Principales

### Flujo 1: Venta Contado
1. Buscar productos → agregar al carrito
2. Aplicar descuentos si aplica
3. Seleccionar forma de pago
4. Entrega: Mostrador/Delivery
5. Generar ticket/factura
6. Actualizar stock

### Flujo 2: Venta "Anotar" (Cliente debe)
1. Buscar productos → agregar al carrito
2. NO se cobra en el momento
3. Seleccionar cliente
4. Guardar como "cuenta pendiente"
5. Mostrar en lista de cobros pendientes
6. Cuando cliente paga → registrar cobro → generar factura

### Flujo 3: Preventa/Pedido
1. Cliente pide merchandise → se anota
2. Cuando llega mercadería se notifica
3. Cliente retira y paga → se cobra → se factura

### Flujo 4: Cobro de Deuda
1. Ver lista de clientes con deuda
2. Seleccionar cliente
3. Ver facturas pendientes
4. Registrar pago (parcial/total)
5. Aplicar a facturas específicas
6. Actualizar estado

---

## 6. Consideraciones de Crecimiento

- **Multi-sucursal:** Diseñado para escalar a varias sucursales
- **Mayorista:** Precios especiales por cliente
- **Delivery:** Seguimiento de pedidos
- **Promociones:** Descuentos por cantidad, fechas
- **Crédito:** Límites y control de mora
- **Integraciones:** Potential integración con contabilidad

---

## 7. Priority Features (MVP)

### Fase 1 - Esencial:
1. Catálogo de productos con códigos de barras
2. Punto de venta con cart
3. Control de stock automático
4. Clientes con deuda
5. "Anotar" y cobrar después
6. Reportes básicos de ventas

### Fase 2 - Importante:
1. Facturación fiscal
2. Cuentas por pagar
3. Reportes avanzados
4. Permisos de usuarios

### Fase 3 - Crecimiento:
1. App móvil
2. Multi-sucursal
3. Integraciones
4. Dashboard analítico
