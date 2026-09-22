# 📊 Informe de Ejecución de Pruebas · Ciclo 1

| Campo | Detalle |
|---|---|
| **Versión probada** | SauceDemo (producción, septiembre 2026) |
| **Ciclo** | 1 · Regresión completa |
| **Fecha** | 22 de septiembre de 2026 |
| **Ejecutado por** | Yaquelin Rugel |
| **Ambiente** | Local (Windows) + CI (GitHub Actions, ubuntu-latest) |
| **Navegadores** | Chromium · Firefox · WebKit · Pixel 7 (móvil) |

---

## 1. Resumen ejecutivo

✅ **Se recomienda aprobar la versión para `standard_user`**: el 100 % de los casos de prioridad Alta pasó en los 4 navegadores
y no hay defectos abiertos en el flujo principal de compra.

⚠️ Se reportan **2 defectos** que afectan solo al perfil `problem_user`. Uno de ellos (**BUG-02**, severidad Alta) bloquea la compra
para ese perfil y debe corregirse antes de habilitarlo.

## 2. Resultados de la ejecución

| Métrica | Valor |
|---|---|
| Casos diseñados | 30 |
| Casos ejecutados | 30 (100 %) |
| Aprobados | 30 |
| Fallidos | 0 |
| Bloqueados | 0 |
| Defectos nuevos | 2 (BUG-01, BUG-02) |

### Ejecución automatizada (CI)

| Navegador | Casos | Resultado | Duración |
|---|---|---|---|
| Chromium | 29 | ✅ Aprobado | ~49 s |
| Firefox | 29 | ✅ Aprobado | ~1 min 15 s |
| WebKit (Safari) | 29 | ✅ Aprobado | ~1 min 33 s |
| Mobile Chrome (Pixel 7) | 29 | ✅ Aprobado | ~49 s |
| **Total** | **116 ejecuciones** | **✅ 100 %** | |

> Los 2 casos de defectos conocidos (BUG-01 y BUG-02) están marcados con `test.fail()`: la suite los ejecuta, **confirma que el bug sigue presente**
> y los cuenta como resultado esperado. Si el defecto se corrige, la prueba avisará para retirar la marca.

### Ejecución manual

| Caso | Descripción | Resultado esperado | Resultado obtenido | Estado |
|---|---|---|---|---|
| CART-07 | Carrito con 5 productos (valor límite máx − 1) | Contador = 5 | El contador mostró 5 | ✅ Aprobado |
| CART-08 | Carrito con los 6 productos (valor límite máx) | Contador = 6 y todos los botones en "Remove" | El contador mostró 6 | ✅ Aprobado |
| CHK-01 (`problem_user`) | Checkout con el perfil `problem_user` | El apellido se escribe en *Last Name* | Cada tecla reemplaza *First Name* → [BUG-02](bugs/BUG-02-apellido-no-editable.md) | ❌ Defecto |

📈 Reporte Allure en vivo: https://yaquelin2305.github.io/playwright-e2e-saucedemo/

### Resultados por módulo

| Módulo | Casos | Aprobados | Defectos |
|---|---|---|---|
| Autenticación | 7 | 7 | – |
| Catálogo | 6 | 6 | BUG-01 (`problem_user`) |
| Carrito | 8 | 8 | – |
| Checkout | 6 | 6 | BUG-02 (`problem_user`) |
| Accesibilidad | 2 | 2 | – |
| Compatibilidad móvil | 1 | 1 | – |

## 3. Defectos

| ID | Título | Severidad | Prioridad | Estado |
|---|---|---|---|---|
| [BUG-01](bugs/BUG-01-imagenes-duplicadas.md) | Todos los productos muestran la misma imagen | Media | P2 | Abierto |
| [BUG-02](bugs/BUG-02-apellido-no-editable.md) | El apellido se escribe en "First Name" y bloquea la compra | Alta | P1 | Abierto |

## 4. Incidencias de la automatización (lecciones aprendidas)

| Incidencia | Causa raíz | Solución |
|---|---|---|
| LOGIN-02 fallaba por *timeout* (30 s) en la primera ejecución local | El selector `data-test="open-menu"` apuntaba a la **imagen** del ícono, tapada por un `<button>` transparente. Playwright detectaba que otro elemento recibiría el clic y esperaba hasta el *timeout*. | Se cambió el localizador a `getByRole('button', { name: 'Open Menu' })`: se hace clic en el elemento real con el que interactúa el usuario. Ahora pasa en 2,2 s. |

**Lección:** no fue un defecto del producto, sino del script. Antes de reportar un bug hay que confirmar que la falla se reproduce
manualmente, y conviene preferir localizadores por **rol accesible**.

## 5. Criterios de salida

| Criterio | Estado |
|---|---|
| 100 % de casos de prioridad Alta aprobados | ✅ Cumple |
| ≥ 95 % del total de casos aprobados | ✅ Cumple (100 %) |
| 0 defectos Críticos/Altos abiertos en el flujo de `standard_user` | ✅ Cumple |
| Defectos restantes documentados y priorizados | ✅ Cumple |

## 6. Recomendaciones

1. Corregir **BUG-02** (P1) antes de habilitar el perfil afectado.
2. Agregar validación de formato al código postal (mejora; hoy acepta cualquier texto).
3. Automatizar CART-07 y CART-08 (valores límite del carrito, hoy aprobados de forma manual) en el próximo ciclo.
4. Medir el tiempo de respuesta del login con `performance_glitch_user` e incorporar pruebas de rendimiento.
