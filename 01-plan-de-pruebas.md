# 📋 Plan de Pruebas · SauceDemo (Swag Labs)

| Campo | Detalle |
|---|---|
| **ID del documento** | PP-SAUCE-001 |
| **Versión** | 1.0 |
| **Autora** | Yaquelin Rugel Alvarado, QA Analyst |
| **Fecha** | Septiembre 2026 |
| **Estándar de referencia** | ISO/IEC/IEEE 29119-3 (estructura adaptada) · Glosario ISTQB |
| **Sistema bajo prueba (SUT)** | [SauceDemo](https://www.saucedemo.com), tienda e-commerce web de práctica |

---

## 1. Introducción y objetivo

Este plan define **qué se va a probar, cómo, con qué criterios y con qué recursos** para validar los flujos críticos
de compra de la tienda SauceDemo antes de una liberación a producción.

**Objetivos:**
1. Verificar que un cliente pueda autenticarse, elegir productos y completar una compra sin errores.
2. Validar las reglas de negocio: cálculo de subtotal, impuesto y total.
3. Detectar y documentar defectos, clasificados por severidad y prioridad.
4. Dejar una suite de regresión automatizada que corra en cada cambio (CI/CD).

## 2. Alcance

### 2.1 Dentro del alcance

| Módulo | Funcionalidades | Requisitos |
|---|---|---|
| Autenticación | Login, mensajes de error, logout, protección de rutas | REQ-01, REQ-02 |
| Catálogo | Listado, ordenamiento, detalle de producto | REQ-03, REQ-04 |
| Carrito | Agregar, quitar, contador, persistencia | REQ-05 |
| Checkout | Datos del cliente, resumen con totales, confirmación | REQ-06, REQ-07 |
| Accesibilidad | Cumplimiento WCAG 2.1 A/AA en login y catálogo | REQ-08 |

### 2.2 Fuera del alcance

- Pasarela de pago real (el sitio usa un medio de pago simulado).
- Pruebas de seguridad (penetración, inyección SQL) y de carga masiva.
- Backend/APIs: SauceDemo es una aplicación *front-end only*.
- Enlaces externos (redes sociales, página "About").

## 3. Estrategia y enfoque

| Nivel / tipo | Enfoque | Herramienta |
|---|---|---|
| **Pruebas funcionales de sistema** | Casos diseñados con técnicas de caja negra (ver [03-tecnicas-de-diseno.md](03-tecnicas-de-diseno.md)) | Manual + Playwright |
| **Pruebas negativas** | Entradas inválidas, campos vacíos, usuarios bloqueados | Playwright (data-driven) |
| **Pruebas E2E** | Flujo completo login → compra → confirmación | Playwright |
| **Regresión** | Toda la suite automatizada en cada *push* y cada noche | GitHub Actions |
| **Compatibilidad** | Chromium, Firefox, WebKit (Safari) y móvil (Pixel 7) | Playwright projects |
| **Accesibilidad** | Reglas automáticas WCAG A/AA | axe-core |
| **Exploratorias** | Sesiones de 30 min con *charter* por módulo | Manual |

**Priorización basada en riesgo:** los casos se priorizan según su impacto en el negocio (¿impide vender?) y la probabilidad de falla.
El flujo de compra y el login son **prioridad Alta** y forman parte del set `@smoke`.

## 4. Técnicas de diseño aplicadas

- Partición de equivalencia (login, formulario de checkout)
- Análisis de valores límite (cantidad de productos en el carrito)
- Tabla de decisión (combinaciones de usuario/contraseña)
- Transición de estados (ciclo del carrito y del checkout)
- Pruebas basadas en la experiencia: *error guessing* y sesiones exploratorias

Detalle y ejemplos en [03-tecnicas-de-diseno.md](03-tecnicas-de-diseno.md).

## 5. Ambiente de pruebas

| Elemento | Detalle |
|---|---|
| URL | https://www.saucedemo.com |
| Navegadores | Chrome/Chromium, Firefox, Safari/WebKit (versiones incluidas en Playwright) |
| Dispositivo móvil | Emulación Pixel 7 |
| Datos de prueba | Usuarios provistos por el sitio: `standard_user`, `locked_out_user`, `problem_user`, `performance_glitch_user`, `error_user`, `visual_user` · contraseña `secret_sauce` |
| CI | GitHub Actions · ubuntu-latest |
| Gestión de casos y bugs | Formato compatible con Jira + Xray (CSV importable en [casos-de-prueba/](casos-de-prueba/)) |

## 6. Criterios de entrada y salida

**Criterios de entrada** (para comenzar a probar):
- El ambiente está disponible y responde.
- Los requisitos/historias de usuario están definidos y revisados.
- Los datos de prueba (usuarios) están disponibles.

**Criterios de salida** (para dar la versión por aprobada):
- 100 % de los casos de prioridad **Alta** ejecutados y aprobados.
- ≥ 95 % de todos los casos ejecutados aprobados.
- 0 defectos abiertos de severidad **Crítica** o **Alta** en el flujo de `standard_user`.
- Defectos restantes documentados, priorizados y aceptados por el Product Owner.

**Criterios de suspensión:** el ambiente no está disponible o un defecto bloqueante impide ejecutar más del 30 % de los casos.

## 7. Riesgos y mitigación

| ID | Riesgo | Prob. | Impacto | Mitigación |
|---|---|---|---|---|
| R-01 | El sitio de práctica cambia sus selectores | Media | Alto | Uso de atributos `data-test` estables y Page Object Model (un solo punto de cambio) |
| R-02 | Pruebas inestables (*flaky*) por tiempos de carga | Media | Medio | Esperas automáticas de Playwright y reintentos en CI (2) |
| R-03 | El sitio no está disponible | Baja | Alto | Ejecución nocturna y reintento manual; reporte del incidente |
| R-04 | Diferencias entre navegadores | Media | Medio | Ejecución en 4 proyectos de navegador |

## 8. Entregables

| Entregable | Ubicación |
|---|---|
| Plan de pruebas | Este documento |
| Requisitos y criterios de aceptación | [02-requisitos.md](02-requisitos.md) |
| Diseño de pruebas | [03-tecnicas-de-diseno.md](03-tecnicas-de-diseno.md) |
| Casos de prueba (CSV para Xray) | [casos-de-prueba/matriz-casos-de-prueba.csv](casos-de-prueba/matriz-casos-de-prueba.csv) |
| Matriz de trazabilidad | [casos-de-prueba/matriz-trazabilidad.md](casos-de-prueba/matriz-trazabilidad.md) |
| Reportes de defectos | [bugs/](bugs/) |
| Informe de ejecución | [04-informe-de-ejecucion.md](04-informe-de-ejecucion.md) |
| Suite automatizada | [playwright-e2e-saucedemo](https://github.com/yaquelin2305/playwright-e2e-saucedemo) |

## 9. Roles y responsabilidades

| Rol | Responsabilidad |
|---|---|
| QA Analyst | Planificación, diseño y ejecución de pruebas, automatización, reporte de defectos |
| Desarrollo | Corrección de defectos y soporte en el análisis de causa raíz |
| Product Owner | Priorización de defectos y aprobación de la salida |

## 10. Clasificación de defectos

| Severidad | Definición |
|---|---|
| **Crítica** | El sistema se cae o hay pérdida de datos. Sin alternativa. |
| **Alta** | Una funcionalidad principal no funciona (p. ej., no se puede comprar). |
| **Media** | La funcionalidad opera con errores o hay una alternativa disponible. |
| **Baja** | Problema visual o de texto que no afecta el uso. |

La **prioridad** (P1 a P4) la define el Product Owner según la urgencia de negocio. Es independiente de la severidad.
