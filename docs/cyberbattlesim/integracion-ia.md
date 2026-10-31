# Integración con IA

```text
observation → agente → action → env.step() → reward + nueva observation
```

## Agente RL

```text
Gym observation → RL → Gym action
```

El agente transforma la observación en características si lo necesita y aprende a seleccionar acciones usando las transiciones y recompensas. DQL es un ejemplo existente; véase [Agentes baseline](agentes-baseline.md).

## Posible agente LLM

```text
Gym observation → State Formatter → LLM → Action Mapper → Gym action
```

El **State Formatter** transformaría la observación estructurada en una representación comprensible para el modelo, conservando la distinción entre información conocida y desconocida. Por ejemplo:

```text
Node 0 controlado. Node 1 descubierto.
Node 1 parece Linux. Existe una credencial SSH para Node 1.
```

El **LLM** podría proponer: «Conectarse mediante SSH a Node 1 usando la credencial conocida». El **Action Mapper** traduciría la decisión a una acción compatible, por ejemplo `{"connect": [...]}`, resolviendo índices y comprobando su validez. La máscara no garantiza éxito. Estas piezas son una propuesta conceptual, no clases implementadas.

## Posible agente híbrido LLM + RL

Se pueden estudiar varias combinaciones sin elegir aún una arquitectura final:

- LLM propone la estrategia y RL selecciona la acción.
- LLM propone acciones candidatas y RL elige una.
- RL propone una acción y LLM la evalúa.

## Qué resuelve Gymnasium

La interfaz proporciona inicio de episodios, observaciones, espacio de acciones, ejecución, recompensas, finalización e interfaz común para distintos agentes. Los detalles concretos dependen de CyberBattleSim y de sus wrappers; véase [Gymnasium](gymnasium.md).

La aportación del TFG se centra en la representación del estado, el diseño de agentes, la integración con LLM, una posible combinación LLM + RL, la experimentación y la comparación reproducible.
