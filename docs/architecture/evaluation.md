# Evaluación de un vertical

Un vertical no está "listo" cuando el código funciona. Está listo cuando se puede demostrar, contra una vara definida de antemano, que hace lo que el dominio del problema exige. Este documento describe cómo se evalúa un vertical en MNC AgentOS. No contiene las rúbricas de ningún asistente real —esas son parte del activo de dominio—; describe el **método** por el que se construyen y se aplican.

## Principio rector

La calidad de un sistema de IA está acotada por la calidad de su evaluación, no por la del modelo. Un modelo excelente bajo una evaluación pobre produce un sistema del que no se puede afirmar nada: no porque falle, sino porque no hay forma de saber si falla. La evaluación es lo que convierte una intuición ("anda bien") en una afirmación defendible. Por eso, en este proyecto, **sin rúbrica no hay "listo"**: la ausencia de criterio no es un estado neutro, es la imposibilidad de decidir.

## Verificación y Validación no son lo mismo

La distinción viene de la ingeniería de sistemas y, aplicada a agentes, separa dos preguntas que es fácil confundir:

- **Verificación — ¿construimos el agente bien?** El agente cumple su contrato: respeta su constitución, ejecuta sus skills según su especificación, mantiene su tono, activa la escalación cuando corresponde. Se mide con tests de comportamiento, repetibles y automatizables. Vive en el dominio de la solución.
- **Validación — ¿construimos el agente correcto?** El agente resuelve el problema real del usuario en su contexto real. Se mide contra casos reales —no sintéticos—, observando si la persona logra lo que necesitaba. Vive en el dominio del problema.

Son ortogonales. Un agente puede estar perfectamente verificado y no validado: cumple cada línea de su spec y, aun así, no le sirve a nadie. La inversa también ocurre: resuelve el problema en una demo pero viola sus propios límites bajo presión. Ambos bucles tienen que cerrar.

## Las rúbricas se definen antes de construir

Los criterios de éxito de un vertical —cuántos, medibles, sin ambigüedad— se derivan del **dominio del problema** y se escriben **antes** de la implementación, no después. Esto no es burocracia: es lo que mantiene honesta a la evaluación. Una rúbrica escrita después de ver funcionar el sistema tiende a describir lo que el sistema ya hace, no lo que el usuario necesita. Definirla antes la ancla al problema.

El patrón es siempre el mismo, independientemente del vertical: traducir cada criterio de éxito declarado en la constitución a un chequeo concreto y observable. Cómo se llenan esos chequeos con contenido de dominio es propio de cada asistente y no se documenta acá. Las rúbricas son, recordemos, el tercer artefacto del [patrón vertical](vertical-pattern.md): constitución, skills y rúbricas.

## La evaluación es un contrato temprano

Definir cómo se medirá el éxito de un requisito, en el momento de escribirlo, hace algo más que preparar la prueba: **valida el requisito mismo.** Si un criterio de éxito no se puede expresar como algo comprobable, es señal de que está ambiguo o mal planteado, y conviene saberlo antes de construir y no después. La evaluación temprana da trazabilidad —cada criterio se puede seguir desde el problema que lo originó hasta el chequeo que lo verifica— y convierte la vara en un contrato entre el problema y la solución.

## Un anti-patrón concreto

> El conjunto de tests da 100% verde y el usuario abandona la conversación a los tres minutos.

Es el caso de libro de un sistema **verificado pero no validado**. Cada test pasa porque el agente hace exactamente lo que la spec dice; el usuario se va porque la spec no resolvía su problema. Mirar solo el tablero verde habría declarado el vertical "listo". Por eso ambos bucles importan: la verificación protege de construir mal; la validación protege de construir lo incorrecto. Un vertical se considera listo solo cuando los dos cierran.
