# MNC AgentOS

Arquitectura de referencia para una plataforma de **asistentes verticales** construida sobre un único motor de agentes reutilizable. Este repositorio documenta el **diseño** —no la implementación de los productos— bajo un modelo **open-core** y **local-first**.

> Repositorio de diseño. No contiene constituciones de producción, corpus curados, evaluaciones específicas ni código de verticales. Esos artefactos son el activo competitivo y viven fuera de aquí.

## Tesis

La mayoría de los asistentes con IA se construyen como aplicaciones monolíticas: cada nuevo dominio reescribe orquestación, memoria, enrutamiento de modelos y conexión a canales. Ese acoplamiento hace que el conocimiento de dominio —lo único difícil de replicar— quede enterrado en código de plomería.

MNC AgentOS invierte esa relación. El motor es genérico, estable y reutilizable; el valor de cada asistente se expresa como **configuración declarativa** sobre ese motor. Un "vertical" no es un fork ni un servicio nuevo: es un paquete de tres artefactos —una **constitución**, un conjunto de **skills** y un conjunto de **rúbricas de evaluación**— que el mismo motor interpreta. Ver [docs/architecture/vertical-pattern.md](docs/architecture/vertical-pattern.md).

## Principios

- **Open-core.** El motor se publica bajo MIT para atraer adopción y feedback. La diferenciación no está en el runtime sino en el conocimiento de dominio que cada vertical encapsula.
- **Local-first.** La memoria y el estado residen primero en el dispositivo o servidor del operador, no en un servicio remoto. La nube es un acelerador opcional, no una dependencia dura.
- **Configuración sobre código.** Lanzar un asistente nuevo es escribir su constitución, sus skills y sus rúbricas —no extender el motor. El motor solo cambia cuando aparece una responsabilidad transversal genuinamente nueva.
- **Decisiones auditables.** Toda elección arquitectónica relevante queda registrada como ADR en [docs/decisions/](docs/decisions/). El "por qué" se versiona junto al "qué".

## Mapa del repositorio

```
mnc-agentos/
├── docs/
│   ├── architecture/
│   │   ├── engine-core.md        # Responsabilidades del motor (nivel interfaz)
│   │   ├── vertical-pattern.md   # El patrón vertical = constitución + skills + rúbricas
│   │   └── evaluation.md         # Cómo se evalúa un vertical (Verificación vs Validación)
│   └── decisions/
│       ├── TEMPLATE.md           # Plantilla de ADR
│       ├── 0001-engine-core.md   # Adoptar un motor OSS único vs construir desde cero
│       └── 0002-aislamiento-proveedores.md  # Aislar proveedores tras una capa de adaptación
├── CONTRIBUTING.md
├── ROADMAP.md                    # Direccional: hacia dónde va la plataforma
└── LICENSE                       # MIT
```

## Cómo leer este repo

1. Empezá por [docs/architecture/engine-core.md](docs/architecture/engine-core.md) para entender qué responsabilidades concentra el motor y cuáles delega.
2. Seguí con [docs/architecture/vertical-pattern.md](docs/architecture/vertical-pattern.md) para ver cómo un dominio concreto se expresa como configuración.
3. Continuá con [docs/architecture/evaluation.md](docs/architecture/evaluation.md) para ver cómo se decide que un vertical está "listo": las rúbricas son el tercer artefacto del patrón, y se entienden mejor después de leerlo.
4. Revisá [docs/decisions/0001-engine-core.md](docs/decisions/0001-engine-core.md) y [docs/decisions/0002-aislamiento-proveedores.md](docs/decisions/0002-aislamiento-proveedores.md) como ejemplos del formato de decisión que usa el proyecto.
5. Cerrá con [ROADMAP.md](ROADMAP.md) para ver hacia dónde apunta la plataforma.

## Licencia

MIT. Ver [LICENSE](LICENSE).
