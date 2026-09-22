# 🔗 Matriz de Trazabilidad

Relaciona cada **requisito** con sus **casos de prueba**, su **automatización** y los **defectos** encontrados.
Sirve para asegurar que ningún requisito quede sin probar y para medir la cobertura.

| Requisito | Descripción | Casos de prueba | Automatizados | Defectos | Cobertura |
|---|---|---|---|---|---|
| REQ-01 | Inicio de sesión | LOGIN-01, 03, 04, 05, 06, 07 | 6/6 | – | ✅ 100 % |
| REQ-02 | Logout y protección de rutas | LOGIN-02 | 1/1 | – | ✅ 100 % |
| REQ-03 | Catálogo de productos | INV-01, INV-06 | 2/2 | [BUG-01](../bugs/BUG-01-imagenes-duplicadas.md) | ✅ 100 % |
| REQ-04 | Ordenamiento | INV-02, 03, 04, 05 | 4/4 | – | ✅ 100 % |
| REQ-05 | Carrito | CART-01 … 08, MOB-01 | 7/9 | – | ✅ 100 % (2 manuales) |
| REQ-06 | Datos del cliente | CHK-01, 04, 05, 06 | 4/4 | [BUG-02](../bugs/BUG-02-apellido-no-editable.md) | ✅ 100 % |
| REQ-07 | Resumen, totales y confirmación | CHK-01, 02, 03, MOB-01 | 4/4 | – | ✅ 100 % |
| REQ-08 | Accesibilidad | A11Y-01, A11Y-02 | 2/2 | – | ✅ 100 % |

## Resumen

| Métrica | Valor |
|---|---|
| Requisitos | 8 |
| Requisitos con al menos un caso | 8 (100 %) |
| Casos de prueba diseñados | 30 |
| Casos automatizados | 28 (93 %) |
| Casos manuales | 2 (CART-07, CART-08) |
| Defectos reportados | 2 |

## Casos por prioridad

| Prioridad | Cantidad |
|---|---|
| Alta | 8 |
| Media | 18 |
| Baja | 4 |

📄 El detalle de cada caso (precondiciones, pasos y resultado esperado) está en
[matriz-casos-de-prueba.csv](matriz-casos-de-prueba.csv). Ese archivo se puede importar en **Jira + Xray**
(*Test Case Importer → CSV*) o abrir en Excel.
