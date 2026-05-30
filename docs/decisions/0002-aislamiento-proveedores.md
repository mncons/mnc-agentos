# 0002 — Aislar la plataforma de proveedores externos mediante capas de adaptación

- **Estado:** Aceptada
- **Fecha:** 2026-05-30
- **Decisores:** Arquitectura de plataforma

## Contexto

La plataforma depende de tres clases de proveedor externo: modelos de lenguaje, infraestructura de cómputo/hosting y canales de mensajería. Acoplar el sistema a un proveedor concreto en cualquiera de esas clases introduce un riesgo que no es de implementación sino estructural, y dos fuerzas lo vuelven inaceptable.

La primera es económica. La inferencia en nube tiene costo variable que escala con el uso: cada conversación adicional cuesta, y ese costo crece de forma monótona con la adopción. La inferencia local, en cambio, es CAPEX —una inversión de hardware una sola vez— con costo marginal decreciente a medida que el uso amortiza la inversión. Una plataforma que aspira a operar muchos verticales no puede tener su estructura de costos atada al camino más caro por defecto.

La segunda es de soberanía y continuidad. Un bloqueo de cuenta, un cambio unilateral de términos, una subida de precios o la discontinuación de un servicio no deben poder detener la operación. La continuidad del servicio al usuario no puede quedar a merced de una decisión que se toma fuera de la organización.

## Decisión

Todo proveedor externo —modelo, hosting o canal— se integra **detrás de una capa de adaptación (Anti-Corruption Layer)** que traduce el lenguaje del proveedor al dominio de MNC. El sistema habla siempre su propio vocabulario interno; el adaptador absorbe las particularidades de cada proveedor en su frontera. La consecuencia operativa es directa: **cambiar de proveedor es reescribir un adaptador, no el sistema.**

Sobre esa base, se prioriza la capacidad de **correr local** —modelos de pesos abiertos— como opción siempre disponible. La nube es un acelerador opcional, no una dependencia dura: se usa cuando aporta capacidad o conveniencia, pero el sistema debe poder degradar a operación local sin reescribirse. Esta capa de aislamiento es la que materializan el enrutamiento de modelos y la abstracción de canales descritos en [engine-core.md](../architecture/engine-core.md).

Proveedores concretos, modelos locales soportados y cualquier cifra de costo: <!-- TODO: dato real -->

## Alternativas consideradas

La decisión se evaluó como un trade-off entre disciplina de diseño y velocidad inicial.

- **Acoplarse a un solo proveedor** — máxima velocidad inicial y mínimo código de integración. Descartada: produce lock-in y traslada el riesgo de continuidad de la plataforma a un tercero. El ahorro de hoy se paga como fragilidad estructural mañana.
- **Multi-proveedor sin capa de aislamiento** — usar varios proveedores pero integrándolos directamente en la lógica del sistema. Descartada: el lenguaje de cada proveedor se filtra al núcleo, de modo que cambiar o agregar uno se vuelve costoso y propenso a error. Tener opciones no sirve si ejercerlas exige cirugía mayor.
- **Capa de adaptación (ACL) + opción local (elegido)** — cada proveedor vive detrás de un adaptador y siempre existe una ruta local. Mayor costo de diseño por integración, a cambio de que el cambio de proveedor sea barato y la operación, soberana.

## Consecuencias

Se gana **optionality estratégica**: la capacidad de cambiar de proveedor, negociar desde una posición sin lock-in, o degradar a local ante un corte, es una propiedad del diseño y no una esperanza. La economía mejora a escala porque la ruta local acota el costo variable.

El costo es disciplina y mantenimiento: cada integración nueva cuesta escribir y sostener un adaptador, y la capa de traducción agrega una indirección que hay que mantener correcta. Aceptamos esa deuda conscientemente; es el precio de no quedar atados. Revisaríamos esta decisión solo si el costo de mantener los adaptadores superara, de forma sostenida, el valor de la optionality que compran —algo que no anticipamos mientras la plataforma siga operando múltiples verticales.
