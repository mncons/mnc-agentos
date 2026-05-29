# Contribuir

Este es un repositorio de **diseño**, no de implementación. Lo que se versiona acá son decisiones de arquitectura y la documentación del patrón sobre el que se construye la plataforma. No contiene código de productos, constituciones de producción, corpus ni evaluaciones específicas —esos artefactos viven fuera de este repo por diseño.

En consecuencia, contribuir significa, casi siempre, **proponer o discutir una decisión de arquitectura**, no abrir un pull request con código.

## Proponer un cambio de arquitectura

Toda decisión arquitectónica relevante se registra como un **ADR** (Architecture Decision Record) en [`docs/decisions/`](docs/decisions/). El flujo es:

1. Copiá [`docs/decisions/TEMPLATE.md`](docs/decisions/TEMPLATE.md) a `docs/decisions/NNNN-titulo-corto.md`, con el siguiente número secuencial de cuatro dígitos.
2. Completá el contexto antes que la decisión: si el contexto está bien planteado, la decisión casi se escribe sola. Documentá las alternativas reales con su motivo de descarte y las consecuencias —incluidas las negativas.
3. Si te falta un dato que aún no tenés, dejá `<!-- TODO: dato real -->` en lugar de inventarlo. Una decisión con un hueco honesto es mejor que una con un número fabricado.
4. Abrí el cambio para discusión con el ADR en estado `Propuesta`. Se mueve a `Aceptada` cuando hay acuerdo.

Las decisiones no se borran ni se editan en silencio: cuando una decisión reemplaza a otra, la anterior se marca como `Reemplazada por NNNN`. El historial del *por qué* es tan valioso como el estado actual.

## Cambios a la documentación del patrón

Las mejoras a `docs/architecture/` son bienvenidas mientras respeten la frontera del proyecto: describen el motor a nivel de **interfaz y responsabilidad**, y el patrón vertical a nivel de **forma**. No incorpores contenido de dominio concreto —tono real de un asistente, reglas de negocio, fragmentos de corpus o rúbricas de producción—. Si una mejora necesita ese contenido para entenderse, probablemente pertenezca a un repo privado, no a este.
