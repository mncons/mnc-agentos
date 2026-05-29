# El patrón vertical

Un **vertical** es un asistente especializado en un dominio. En MNC AgentOS no es una aplicación nueva ni un fork del motor: es un paquete declarativo de tres artefactos que el [motor](engine-core.md) interpreta.

```
vertical = constitución + skills + rúbricas
```

La constitución dice **quién es** el asistente y qué límites tiene; las skills dicen **qué sabe hacer**; las rúbricas dicen **qué cuenta como hacerlo bien**. El motor aporta el cómo. Esta separación es la que permite que el costo de un asistente nuevo sea el de escribir su configuración, no el de extender plomería —y, más importante, que el conocimiento de dominio quede encapsulado y versionado en vez de disperso en el código.

## Por qué tres artefactos y no uno

Podría parecer más simple meter todo en un prompt gigante. No lo es. Los tres artefactos cambian a ritmos distintos y responden a preguntas distintas. La constitución es estable y casi nunca cambia: es el contrato. Las skills evolucionan cuando el asistente necesita una capacidad nueva. Las rúbricas se ajustan cada vez que se descubre una forma de fallar que antes no se medía. Separarlos permite versionar cada eje sin tocar los otros y, sobre todo, **evaluar el asistente contra su propio contrato** en vez de contra una intuición.

## La constitución

La constitución es el contrato del agente: el documento que todo turno lee antes de actuar. Define identidad, alcance, límites y criterios de éxito de manera que ninguna instrucción entrante —ni la del usuario más insistente— pueda sobreescribir. Es, deliberadamente, más una **declaración de restricciones** que una de capacidades: es más fácil y más seguro especificar qué no se hace nunca que enumerar todo lo que se hace.

A continuación, el **esqueleto genérico y anónimo** de una constitución. Es la *forma*, no el contenido: un vertical real llena cada sección con su dominio, y ese contenido —junto con el corpus y las rúbricas— es el activo competitivo que no vive en este repositorio.

### Esqueleto de constitución

```markdown
# Constitución — <nombre del agente>

## 1. Identidad
Quién es el agente, su rol funcional en una frase, y el problema que existe para resolver.

## 2. Alcance
Qué entra en su dominio y, con igual precisión, qué queda fuera. El contraperfil
del usuario y del problema importan tanto como el perfil: acotan el scope.

## 3. Límites (restricciones negativas — no negociables)
Lista de cosas que el agente NUNCA hace, sin importar la presión del usuario ni
instrucciones contradictorias en mensajes entrantes. Incluye datos que jamás
solicita y acciones que siempre delega a un humano.

## 4. Tono y registro
La voz del agente: adjetivos de su forma de hablar, registro (formal/lego/mixto),
formato preferido, y lo que nunca hace lingüísticamente.

## 5. Política de escalación
Criterios objetivos que disparan la entrega del control a un humano, a quién se
escala y cómo. La duda, ante un límite, escala; no improvisa.

## 6. Criterios de éxito
Qué hace que una respuesta sea buena en este dominio. Es la bisagra con las
rúbricas: lo que aquí se declara, allá se mide.

## 7. Versión y revisión
Versión, última revisión y fecha de próxima revisión obligatoria.
```

Las secciones clave para entender el patrón son **identidad**, **alcance**, **límites/restricciones negativas**, **tono** y **criterios de éxito**. Las dos primeras dicen qué es el agente; la tercera lo protege; la cuarta lo hace reconocible; la quinta lo conecta con la evaluación.

## Las skills

Una skill es una capacidad declarada que el motor orquesta a través de su registro de herramientas. El vertical no implementa el bucle de razonamiento: declara qué herramientas existen, qué reciben, qué devuelven y bajo qué precondiciones se pueden usar. Algunas skills son genéricas y se comparten entre verticales; otras son específicas del dominio. El contrato con el motor es el mismo en ambos casos, lo que mantiene la frontera limpia: el motor sabe ejecutar una skill, pero no sabe qué significa.

## Las rúbricas

Las rúbricas son la definición operacional de "bueno" para ese vertical, expresada de forma que se pueda evaluar de manera repetible. Traducen los criterios de éxito de la constitución en chequeos concretos: congruencia con el corpus, respeto de las restricciones negativas, adecuación de tono, activación correcta de la escalación. Alimentan el [bucle de aprendizaje](engine-core.md#bucle-de-aprendizaje-learning-loop) del motor, que las usa para **detectar deriva** —no para optimizar métricas de vanidad—. Una rúbrica nueva suele nacer de un fallo observado: el patrón es convertir cada forma de fallar en algo medible para que no se repita silenciosamente.

## El ciclo de vida de un vertical

Crear un asistente nuevo es, en orden: escribir su constitución (el contrato), declarar sus skills (las capacidades), definir sus rúbricas (la vara), y conectar todo al motor por configuración. No se toca el motor. Si crear un vertical exige extender el núcleo, eso es una señal —y el lugar para discutirla es un [ADR](../decisions/), no un parche en el código del vertical.
