# El motor (engine-core)

El motor es la pieza estable y reutilizable de la plataforma. Concentra todo lo que es transversal a cualquier asistente y, deliberadamente, **no sabe nada de dominio**: no conoce a los usuarios de un vertical concreto, ni su tono, ni sus reglas. Esa ignorancia es una propiedad de diseño, no una carencia. Mientras el dominio no se filtre al motor, lanzar un asistente nuevo sigue siendo un acto de configuración y no de ingeniería.

Este documento describe las responsabilidades del motor a nivel de **interfaz y contrato**, no de implementación. Lo que importa aquí es qué garantiza cada componente y dónde están sus costuras de extensión —los puntos por donde un vertical inyecta su comportamiento sin tocar el núcleo.

## Orquestación (runtime)

Es el bucle central: recibe un turno del usuario, decide si responder directamente o invocar una o más herramientas, ejecuta esas herramientas, incorpora sus resultados y vuelve a decidir, hasta producir una respuesta. El motor garantiza que ese bucle termine —con límites de iteración y de presupuesto— y que cada paso sea observable.

La costura de extensión es el **registro de skills**: el vertical declara qué herramientas existen y bajo qué contrato, y el runtime las orquesta sin conocer su semántica. El runtime no decide *qué* hace un asistente; decide *cómo* se ejecuta un turno.

## Memoria

La memoria persiste el estado conversacional y el conocimiento acumulado entre turnos y entre sesiones, bajo la premisa local-first: el almacén primario es del operador. El motor expone una abstracción de lectura/escritura y de olvido —porque olvidar lo que ya no se debe retener es tan parte del contrato como recordar— y deja la política de qué se guarda, por cuánto tiempo y con qué consentimiento en manos del vertical.

El motor garantiza durabilidad y aislamiento entre asistentes; no impone un esquema de qué es "importante". Esa decisión es de dominio.

## Recuperación (retrieval)

Cuando la respuesta debe fundarse en un corpus, la recuperación selecciona el contexto relevante antes de la generación. El motor trata la estrategia de recuperación como **reemplazable**: para un corpus pequeño puede caber entero en el contexto; para uno grande hace falta una estrategia indexada. Lo que el motor fija es la interfaz —"dada una consulta y un corpus, devolvé el contexto pertinente"—, no el algoritmo. Así un vertical elige su estrategia según el tamaño y la forma de su conocimiento, sin que el motor cambie.

## Enrutamiento de modelos (routing)

No todos los turnos merecen el mismo modelo. El enrutamiento decide, por turno o por sub-tarea, qué modelo atiende la petición, escalando desde opciones locales y económicas hacia modelos frontera solo cuando la dificultad lo justifica. El motor define la política como una **cadena de escalado configurable** y mide costo y latencia por tramo; el vertical declara su preferencia de calidad-vs-costo, pero no implementa el ruteo.

El principio: el modelo más caro es el último recurso, no el primero. El enrutamiento existe para que esa disciplina sea automática y auditable.

## Canales

Un asistente debe poder hablar por distintos medios —mensajería, web, API— sin que su lógica dependa del medio. El motor normaliza la entrada y la salida a un formato interno neutral, de modo que agregar un canal es escribir un adaptador, no reescribir el asistente. La lógica del vertical nunca ve el protocolo del canal: ve mensajes.

## Bucle de aprendizaje (learning loop)

El motor cierra el ciclo entre lo que el asistente produjo y lo que debió producir. Registra señales por sesión —no para optimizar engagement, sino para **detectar deriva** respecto de la constitución del vertical— y las expone a las rúbricas de evaluación. El motor provee el mecanismo de captura y realimentación; las rúbricas que definen "bueno" son, otra vez, de dominio.

## La frontera, en una frase

El motor responde *cómo se ejecuta, se recuerda, se recupera, se rutea, se conecta y se evalúa* un turno. El vertical responde *qué se dice, a quién, con qué límites y qué cuenta como éxito*. Mantener esa frontera nítida es lo que hace que la plataforma escale por configuración. Ver [vertical-pattern.md](vertical-pattern.md).
