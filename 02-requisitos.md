# 📌 Requisitos e Historias de Usuario

Requisitos derivados del comportamiento esperado de SauceDemo, escritos como **historias de usuario** con
**criterios de aceptación en Gherkin**. Cada requisito tiene un ID (REQ-xx) que se usa en la
[matriz de trazabilidad](casos-de-prueba/matriz-trazabilidad.md).

---

## REQ-01 · Inicio de sesión

**Como** cliente registrado, **quiero** iniciar sesión con mi usuario y contraseña **para** acceder a la tienda.

```gherkin
Escenario: Login exitoso
  Dado que estoy en la página de login
  Cuando ingreso un usuario válido y su contraseña
  Entonces veo el catálogo de productos

Escenario: Credenciales inválidas
  Dado que estoy en la página de login
  Cuando ingreso una contraseña incorrecta
  Entonces veo el mensaje "Username and password do not match any user in this service"
  Y permanezco en la página de login

Escenario: Usuario bloqueado
  Cuando ingreso con un usuario bloqueado
  Entonces veo el mensaje "Sorry, this user has been locked out."

Escenario: Campos obligatorios
  Cuando dejo vacío el usuario o la contraseña
  Entonces veo un mensaje que indica el campo obligatorio
```

## REQ-02 · Cierre de sesión y protección de rutas

**Como** cliente, **quiero** cerrar sesión **para** que nadie más use mi cuenta en este equipo.

```gherkin
Escenario: Logout
  Dado que inicié sesión
  Cuando abro el menú y elijo "Logout"
  Entonces vuelvo a la página de login

Escenario: Acceso sin sesión
  Dado que no he iniciado sesión
  Cuando intento abrir directamente /inventory.html
  Entonces soy redirigido al login con un mensaje de acceso restringido
```

## REQ-03 · Catálogo de productos

**Como** cliente, **quiero** ver los productos con nombre, descripción, precio e imagen **para** decidir qué comprar.

```gherkin
Escenario: Listado de productos
  Dado que inicié sesión
  Entonces veo 6 productos
  Y cada uno tiene nombre, precio mayor a 0 e imagen propia

Escenario: Detalle de producto
  Cuando hago clic en el nombre de un producto
  Entonces veo su detalle con el mismo nombre y precio que en el catálogo
```

## REQ-04 · Ordenamiento del catálogo

**Como** cliente, **quiero** ordenar los productos **para** encontrarlos más rápido.

```gherkin
Esquema del escenario: Ordenar productos
  Cuando ordeno por "<criterio>"
  Entonces los productos quedan ordenados por <campo> en forma <direccion>

  Ejemplos:
    | criterio                | campo  | direccion   |
    | Name (A to Z)           | nombre | ascendente  |
    | Name (Z to A)           | nombre | descendente |
    | Price (low to high)     | precio | ascendente  |
    | Price (high to low)     | precio | descendente |
```

## REQ-05 · Carrito de compras

**Como** cliente, **quiero** agregar y quitar productos del carrito **para** armar mi compra.

```gherkin
Escenario: Agregar productos
  Cuando agrego un producto
  Entonces el contador del carrito aumenta en 1
  Y el botón del producto cambia a "Remove"

Escenario: Quitar productos
  Dado que tengo productos en el carrito
  Cuando quito uno
  Entonces el contador disminuye en 1
  Y si el carrito queda vacío, el contador no se muestra

Escenario: Persistencia
  Dado que tengo productos en el carrito
  Cuando recargo la página
  Entonces el carrito conserva los mismos productos
```

## REQ-06 · Datos del cliente en el checkout

**Como** cliente, **quiero** ingresar mis datos de envío **para** completar la compra.

```gherkin
Escenario: Campos obligatorios
  Dado que estoy en el paso 1 del checkout
  Cuando dejo vacío el nombre, el apellido o el código postal
  Entonces veo "Error: <Campo> is required"
  Y no avanzo al paso 2
```

## REQ-07 · Resumen, totales y confirmación de compra

**Como** cliente, **quiero** ver el total antes de confirmar **para** saber cuánto voy a pagar.

```gherkin
Escenario: Cálculo de totales
  Dado que estoy en el resumen de compra
  Entonces el subtotal es la suma de los precios de los productos
  Y el impuesto es el 8 % del subtotal, redondeado a 2 decimales
  Y el total es subtotal + impuesto

Escenario: Confirmación
  Cuando hago clic en "Finish"
  Entonces veo "Thank you for your order!"
  Y el carrito queda vacío

Escenario: Cancelar
  Cuando hago clic en "Cancel" en el resumen
  Entonces vuelvo al catálogo y el carrito conserva los productos
```

## REQ-08 · Accesibilidad

**Como** cliente con discapacidad visual o motriz, **quiero** que el sitio sea accesible **para** poder comprar con tecnologías de asistencia.

- Las páginas de login y catálogo no deben tener violaciones de las reglas WCAG 2.1 nivel A y AA detectables automáticamente.
