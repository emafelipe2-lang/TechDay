# AI CODE WARS — Un expediente, una historia

**Proyecto:** Prototipo web de gestión de expedientes ficticios del Bufete Popular  
**Competencia:** TECH DAY UMG BOOT CAMP — Quinta edición, 31 de octubre de 2026  
**Estado actual:** Análisis y planificación inicial. **No se ha implementado ni probado la aplicación.**

## 1. Problema y objetivo

El Bufete Popular ofrece asesoría y asistencia jurídica gratuita a personas de escasos recursos. Para la competencia se propone un prototipo web que permita abrir expedientes ficticios, consultar sus datos, registrar actividades, seguir su evolución y conservar su historial. Los casos y reglas son ficticios; esta aplicación no ofrece asesoría jurídica ni representa procedimientos oficiales.

## 2. Usuarios y prioridades

**Usuario previsto:** personal encargado de registrar, consultar y dar seguimiento a expedientes ficticios del Bufete Popular. Esta caracterización es un supuesto de diseño, no un perfil de usuario especificado por el enunciado.

**Prioridad P0:** cumplir apertura, validación de obligatorios, identificador/fecha/estado automáticos, listado, búsqueda, detalle, actividades, historial, transiciones, cierre, bloqueo y persistencia. **Prioridad secundaria:** mejorar presentación y adaptación a distintas pantallas solo si no compromete P0.

## 3. Alcance mínimo y criterios de aceptación

| ID | Requisito | Prioridad | Criterio de aceptación |
|---|---|---|---|
| RF-001 | Apertura de expediente | P0 | El usuario proporciona alias/nombre ficticio, descripción del asunto y responsable. |
| RF-002 | Validación de obligatorios | P0 | No permite valores vacíos o compuestos solo por espacios; indica el campo a corregir. |
| RF-003 | Datos automáticos | P0 | Genera un identificador único, fecha/hora de apertura y estado `Abierto`. |
| RF-004 | Listado | P0 | Muestra identificador, persona, responsable y estado. |
| RF-005 | Búsqueda | P0 | Encuentra por ID o nombre/alias y muestra aviso sin resultados. |
| RF-006 | Detalle | P0 | Permite consultar datos e historial. |
| RF-007 | Actividades | P0 | Admite descripción obligatoria con fecha/hora automática en `Abierto` y `En seguimiento`; registra cada actividad por separado. |
| RF-008 | Historial | P0 | Presenta secuencia cronológica y distingue actividades de cambios de estado. |
| RF-009 | Inicio de seguimiento | P0 | Solo pasa de `Abierto` a `En seguimiento` por acción explícita. |
| RF-010 | Cierre | P0 | Pasa de `En seguimiento` a `Cerrado` solo con una actividad previa y motivo no vacío ni solo espacios. |
| RF-011 | Expediente cerrado | P0 | Permanece consultable y bloquea actividades y cambios de estado. |
| RF-012 | Registro de cambios | P0 | Guarda apertura, cambios de estado, fecha/hora de cierre y motivo. |
| RF-013 | Persistencia | P0 | Los datos sobreviven a recarga y nueva apertura según instrucciones de uso. |
| RF-014 | Mensajes y navegación | P0 | Presenta confirmaciones, errores e instrucciones comprensibles. |

**Opcionales:** categoría (civil/familiar/laboral), tipo de actividad y filtro por estado.  
**No exigidos:** autenticación, backend, base de datos externa, despliegue público, adjuntos, notificaciones, reportes ni funciones de editar o eliminar.

## 4. Reglas de negocio

1. Los únicos datos obligatorios de ingreso al abrir son persona ficticia/alias, asunto y responsable; usar `trim()` para detectar vacíos, sin eliminar necesariamente el formato del texto original.
2. El expediente inicia en `Abierto` con ID y fecha/hora generados automáticamente.
3. Transiciones permitidas: `Abierto` → `En seguimiento` (acción explícita) y `En seguimiento` → `Cerrado` (al menos una actividad y motivo válido).
4. No hay transición directa `Abierto` → `Cerrado`, reapertura ni modificación de un expediente `Cerrado`.
5. Las actividades se admiten en `Abierto` o `En seguimiento`, pero no en `Cerrado`.
6. Registrar la apertura y cada transición de estado automáticamente en el historial; registrar cada actividad como evento independiente.
7. La consulta de expedientes cerrados sigue disponible.
8. Almacenar exclusivamente datos ficticios. No implementar asesoría jurídica.

## 5. Datos principales (propuesta de diseño, sujeta a aprobación del equipo)

**Expediente:** `id`, `solicitante`, `asunto`, `responsable`, `categoria?`, `fechaApertura`, `estado`, `fechaCierre?`, `motivoCierre?`.  
**Actividad:** `id`, `expedienteId`, `descripcion`, `tipo?`, `fechaHora`.  
**Evento de historial:** `id`, `expedienteId`, `tipoEvento`, `descripcion`, `fechaHora`.

**Supuestos técnicos propuestos, pendientes de validar:** IDs mediante `crypto.randomUUID()` cuando esté disponible; marcas temporales ISO 8601; almacenamiento de expedientes y eventos en una única estructura versionada de `localStorage`; actualización sincronizada de estado e historial para cada operación válida.

## 6. Arquitectura propuesta

- Interfaz web: React + Vite + JavaScript.
- Estilos: CSS simple; evaluar Tailwind solo si no retrasa el objetivo.
- Reglas de negocio: funciones centralizadas para crear, agregar actividades y cambiar estados.
- Persistencia: `localStorage` del navegador, sin backend ni base de datos externa.
- Ejecución: local. El proyecto aún no está inicializado; **no hay instrucciones de ejecución comprobadas** en esta etapa.

La selección de tecnologías es una **propuesta inicial**, no una decisión ya aprobada o verificada por el equipo.

## 7. Responsabilidades e integración

| Rol propuesto | Integrante (completar) | Responsabilidades |
|---|---|---|
| Líder técnico | Pendiente | Arquitectura, reglas y revisión de integración Git. |
| Frontend | Pendiente | Formulario, listado, búsqueda y navegación. |
| Datos e historial | Pendiente | Persistencia, actividades, eventos y consistencia. |
| QA | Pendiente | Casos de prueba, evidencias, defectos y regresión. |
| UX y documentación | Pendiente | Claridad visual, README y demostración. |

**Estrategia de integración propuesta:** usar una rama principal y ramas de trabajo por función cuando sea viable; evitar edición simultánea del mismo archivo; integrar avances funcionales y verificar que el proyecto siga ejecutando antes del siguiente cambio. Publicar commits progresivos: análisis inicial, avances funcionales, correcciones y entrega.

## 8. Plan de 90 minutos efectivos

| Tiempo | Actividad | Evidencia esperada |
|---|---|---|
| 0–12 min | Análisis, asignación de roles y README inicial | Primer commit de análisis. |
| 12–30 min | Estructura, apertura, listado y persistencia | Creación de expedientes operativa. |
| 30–48 min | Búsqueda, detalle, actividades e historial | Recorrido de consulta/actividad. |
| 48–60 min | Estados, cierre, bloqueo y mensajes | Reglas críticas incorporadas. |
| 60–80 min | Pruebas manuales y correcciones | Resultados observados. |
| 80–90 min | README final, revisión, commit y entrega | Enlace y hash final. |

**Límite oficial:** commit final a las 9:30 a. m. La organización anuncia la hora efectiva de inicio.

## 9. Riesgos y mitigación

| Riesgo | Acción de mitigación |
|---|---|
| Tiempo insuficiente | Priorizar P0 y posponer opcionales. |
| Transiciones indebidas | Reglas centralizadas y casos negativos. |
| Historial incompleto | Registrar cada evento junto con la operación correspondiente. |
| Pérdida de datos al recargar | Validar `localStorage` con recarga y reapertura. |
| Conflictos de integración | Responsable por módulo y commits frecuentes. |
| Documentación incompleta | Completar README durante el desarrollo, no solo al final. |

## 10. Tres decisiones relevantes y uso de IA (pendiente de completar con hechos reales)

| Decisión o necesidad | Aporte de la IA | Decisión del equipo y razón | Cómo se comprobó |
|---|---|---|---|
| Tecnología y persistencia | Propuesta inicial: React/Vite y `localStorage`. | **Pendiente de aprobación del equipo.** | **No comprobado.** |
| Modelado de historial | Propuesta inicial: actividades y cambios como eventos cronológicos. | **Pendiente de aprobación del equipo.** | **No comprobado.** |
| Validaciones y transiciones | Propuesta inicial: centralizar reglas y casos positivos/negativos. | **Pendiente de aprobación del equipo.** | **No comprobado.** |

> Actualizar la tabla tras las decisiones y verificaciones efectivamente realizadas. No presentar propuestas como decisiones ya tomadas ni pruebas que aún no ocurrieron.

## 11. Lista de comprobación (todavía no ejecutada)

| Recorrido | Comprobación | Estado | Resultado observado |
|---|---|---|---|
| Apertura | Crear dos expedientes con IDs distintos y rechazar obligatorios inválidos. | Pendiente | No ejecutado. |
| Consulta | Buscar por ID y persona, abrir detalle e historial; probar ausencia de resultados. | Pendiente | No ejecutado. |
| Seguimiento y cierre | Agregar actividades, pasar a seguimiento, cerrar correctamente y rechazar operaciones inválidas. | Pendiente | No ejecutado. |
| Persistencia | Recargar y reabrir sin perder datos. | Pendiente | No ejecutado. |

Al probar, registrar `Cumple`, `Parcial` o `No cumple` y el resultado **realmente observado**.

## 12. Ejecución, datos de demostración y limitaciones

- **Instrucciones de ejecución:** pendientes de implementar y verificar.
- **Dos expedientes ficticios para la demostración:** pendientes de crear durante el desarrollo o las pruebas.
- **Limitaciones conocidas:** antes de la implementación no existen funcionalidades ni pruebas ejecutadas. `localStorage` guarda datos en el navegador/perfil/origen usado; no garantiza sincronización entre dispositivos.

## 13. GitHub y entrega

El repositorio podrá ser público o privado, pero debe ser accesible para ambos catedráticos. Conservar evidencia de tres hitos: (1) ficha inicial y responsabilidades, (2) avances y correcciones durante desarrollo, (3) código, README y pruebas finales. Entregar **URL del repositorio y hash del commit final** antes del cierre oficial.

**Commit inicial sugerido:** `docs: agregar análisis inicial, requisitos y plan de trabajo`.
