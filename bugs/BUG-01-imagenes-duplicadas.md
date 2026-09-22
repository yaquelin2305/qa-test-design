# 🐞 BUG-01 · Todos los productos del catálogo muestran la misma imagen

| Campo | Detalle |
|---|---|
| **ID** | BUG-01 |
| **Estado** | Abierto |
| **Severidad** | Media |
| **Prioridad** | P2 |
| **Módulo** | Catálogo |
| **Requisito afectado** | REQ-03 · Cada producto debe tener su imagen propia |
| **Caso que lo detecta** | INV-01 (manual) · BUG-01 en la suite automatizada |
| **Ambiente** | https://www.saucedemo.com · Chrome, Firefox, Safari (WebKit) · Windows / Ubuntu (CI) |
| **Usuario de prueba** | `problem_user` |
| **Reportado por** | Yaquelin Rugel · Septiembre 2026 |
| **Reproducibilidad** | 100 % (siempre) |

## Resumen
Al iniciar sesión con `problem_user`, las 6 tarjetas del catálogo muestran **la misma imagen** (la foto de un perro)
en lugar de la foto de cada producto.

## Pasos para reproducir
1. Ir a https://www.saucedemo.com
2. Ingresar usuario `problem_user` y contraseña `secret_sauce`
3. Clic en **Login**
4. Observar las imágenes de los productos en el catálogo

## Resultado esperado
Cada producto muestra su propia imagen (mochila, luz de bicicleta, polera, chaqueta, body de bebé y polera roja),
igual que con `standard_user`.

## Resultado obtenido
Los 6 productos muestran la misma imagen. Todas las etiquetas `<img>` apuntan al mismo archivo
(`/assets/sl-404-*.jpg`).

## Evidencia
- Prueba automatizada que lo detecta: [`tests/known-bugs/problem-user.spec.ts`](https://github.com/yaquelin2305/playwright-e2e-saucedemo/blob/main/tests/known-bugs/problem-user.spec.ts) (BUG-01)
- Captura de pantalla: `evidencias/BUG-01.png`

## Impacto
El cliente no puede identificar visualmente los productos, lo que puede llevar a compras equivocadas y afecta la confianza en la tienda.
No bloquea la compra (el nombre y la descripción sí son correctos), por eso la severidad es **Media**.

## Notas para desarrollo
- `standard_user` no presenta el problema, así que la falla depende del perfil de usuario y no del catálogo de imágenes.
- Sugerencia: revisar la lógica que asigna la URL de imagen según el usuario.
