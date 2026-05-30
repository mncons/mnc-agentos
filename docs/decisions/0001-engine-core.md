# 0001 — Adoptar un motor de agentes open source único en lugar de construir desde cero

- **Estado:** Aceptada
- **Fecha:** 2026-05-29
- **Decisores:** Arquitectura de plataforma

## Contexto

La plataforma necesita ejecutar varios asistentes verticales que comparten la misma maquinaria: un bucle de razonamiento con uso de herramientas, memoria con recuperación, enrutamiento entre modelos de distinto costo y capacidad, y conexión a canales de mensajería. Esa maquinaria es transversal: no aporta diferenciación por dominio y, sin embargo, es donde se concentra la mayor parte del riesgo de ingeniería.

Existen tres caminos: construir el motor desde cero, adoptar un framework open source y extenderlo, o componer la plataforma a partir de servicios gestionados de terceros. La decisión está acotada por tres fuerzas: el valor del producto vive en el conocimiento de dominio (constituciones, corpus, rúbricas), no en el runtime; el proyecto es local-first, lo que penaliza fuertemente las dependencias de servicios remotos en el camino crítico; y el equipo es pequeño, lo que vuelve insostenible mantener un motor propio con la velocidad del ecosistema.

## Decisión

Adoptamos un **único motor de agentes open source**, con licencia permisiva, como núcleo reutilizable de la plataforma, y limitamos nuestras intervenciones a una **capa de adaptación delgada** sobre sus puntos de extensión. No construimos un motor propio y no acoplamos el camino crítico a servicios gestionados.

La elección concreta del motor se evalúa contra criterios explícitos y se registrará aquí cuando la prueba de concepto cierre: licencia permisiva compatible con open-core, soporte para enrutamiento multi-modelo (incluidos modelos locales), abstracción de memoria/recuperación reemplazable, neutralidad de canal, calidad del modelo de extensión (skills/tools) y salud de la comunidad. Candidatos en evaluación al momento de este ADR: **Hive** y **Hermes**, evaluación en curso. La selección final y los resultados de la prueba de concepto se registran aquí al cerrar la evaluación.

## Alternativas consideradas

La decisión se evaluó como un trade-off entre cuatro estrategias, contra criterios que actúan como medidas de efectividad (MOE) derivadas del contexto. Los pesos son **cualitativos** —no hay puntajes numéricos porque no existe una fuente verificable para ponderarlos con precisión—, pero el orden de importancia lo fijan las fuerzas del contexto: soberanía y ajuste al patrón vertical pesan más que la economía de corto plazo.

| Criterio (MOE) | Construir desde cero | Adoptar un motor OSS único | Servicios gestionados | Multi-framework |
|---|---|---|---|---|
| Madurez / mantenimiento | Baja — todo a cargo del equipo | Alta — ecosistema activo | Alta — operada por el proveedor | Media — N ecosistemas, N mantenimientos |
| Licencia / open-core | Control total | Permisiva, compatible | Términos del proveedor | Variable por framework |
| Soberanía / continuidad | Alta | Alta — con opción de fork | Baja — dependencia dura | Media |
| Ajuste al patrón vertical | Alto — a costa de construirlo | Alto — capa de adaptación delgada | Bajo — modelo del proveedor | Bajo — frontera distinta por vertical |
| Economía | Mala — CAPEX de ingeniería alto y sostenido | Buena — costo marginal por vertical bajo | Mala a escala — costo variable creciente | Mala — esfuerzo multiplicado |

- **Construir el motor desde cero** — máximo control y cero deuda de dependencias. Descartada: el costo de alcanzar y mantener paridad con el ecosistema OSS supera con creces la diferenciación que aportaría, y desvía al equipo del único trabajo no replicable, que es el de dominio.
- **Componer sobre servicios gestionados de terceros** — time-to-market inmediato y operación mínima. Descartada: contradice el principio local-first, introduce dependencia dura y costo variable en el camino crítico, y cede control sobre datos sensibles del operador (ver [0002 — aislamiento de proveedores](0002-aislamiento-proveedores.md)).
- **Adoptar varios frameworks, uno por vertical** — permitiría elegir la mejor herramienta por caso. Descartada: multiplica la superficie de mantenimiento, rompe la premisa de un motor único reutilizable y hace que el conocimiento operativo no se acumule.
- **Adoptar un motor OSS único (elegido)** — concentra el mantenimiento en un ecosistema sano, preserva soberanía con la opción de fork y mantiene limpia la frontera con el patrón vertical a través de una capa de adaptación delgada. Es la única opción que sale bien parada en los criterios de mayor peso sin hundirse en la economía.

## Consecuencias

Se vuelve más fácil incorporar verticales nuevos: el costo marginal de un asistente cae a escribir su configuración, no a extender plomería. Se reduce el riesgo de ingeniería transversal y se aprovecha el ritmo de mejora del ecosistema OSS.

Se vuelve más difícil divergir del motor: quedamos sujetos a sus decisiones de diseño y a su ciclo de versiones, y asumimos el riesgo de un cambio de licencia o de un abandono del proyecto upstream. Mitigamos esto con la capa de adaptación delgada —que aísla a los verticales de la API concreta del motor— y con el criterio de licencia permisiva, que deja abierta la opción de fork. Aceptamos la deuda de revisar esta decisión si el motor elegido cambia de licencia, es discontinuado, o si una responsabilidad transversal nueva no encaja en sus puntos de extensión.
