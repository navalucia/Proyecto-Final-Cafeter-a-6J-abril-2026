Claro, te doy un ejemplo claro usando **MongoDB (base de datos NoSQL)** para una cafetería ☕. Aquí no se usan tablas como en SQL, sino **colecciones** y **documentos**.

---

# 🗄️ Base de datos: `cafeteria_db`

## 📁 Colección: `clientes`

**Documento ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Lucia Nava",
  "telefono": "6561234567",
  "email": "lucia@gmail.com",
  "fecha_registro": Date,
  "puntos_fidelidad": Number
}
```

**Atributos y tipos:**

* nombre → String
* telefono → String
* email → String
* fecha_registro → Date
* puntos_fidelidad → Number

---

## 📁 Colección: `productos`

**Documento ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Café Latte",
  "descripcion": "Café con leche espumada",
  "precio": Number,
  "categoria": "Bebida",
  "disponible": Boolean
}
```

**Atributos:**

* nombre → String
* descripcion → String
* precio → Number
* categoria → String
* disponible → Boolean

---

## 📁 Colección: `empleados`

**Documento ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Juan Pérez",
  "puesto": "Barista",
  "salario": Number,
  "fecha_contratacion": Date,
  "activo": Boolean
}
```

**Atributos:**

* nombre → String
* puesto → String
* salario → Number
* fecha_contratacion → Date
* activo → Boolean

---

## 📁 Colección: `pedidos`

**Documento ejemplo:**

```json
{
  "_id": ObjectId,
  "cliente_id": ObjectId,
  "empleado_id": ObjectId,
  "fecha": Date,
  "total": Number,
  "estado": "completado",
  "productos": [
    {
      "producto_id": ObjectId,
      "cantidad": Number,
      "precio_unitario": Number
    }
  ]
}
```

**Atributos:**

* cliente_id → ObjectId (referencia a clientes)
* empleado_id → ObjectId (referencia a empleados)
* fecha → Date
* total → Number
* estado → String
* productos → Array de objetos

---

## 📁 Colección: `proveedores`

**Documento ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Distribuidora Café MX",
  "telefono": "6569876543",
  "direccion": "Ciudad Juárez",
  "email": "proveedor@cafe.com"
}
```

---

# 🔗 Relación entre colecciones

* `pedidos.cliente_id` → `clientes._id`
* `pedidos.empleado_id` → `empleados._id`
* `productos` se relaciona dentro de `pedidos.productos`

---

# 🧠 Nota rápida

* MongoDB es **flexible**, no necesitas definir estructura fija.
* Puedes **anidar datos** (como los productos dentro de pedidos).
* Ideal para sistemas como cafeterías porque los pedidos pueden variar.

---

Si quieres, puedo hacerte también este mismo ejemplo pero en **SQL (MySQL con tablas)** o ayudarte a convertirlo en código para MongoDB Compass o Node.js.
