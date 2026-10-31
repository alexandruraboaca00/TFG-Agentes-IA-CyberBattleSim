# Agentes baseline

Las pruebas funcionales del TFG ya ejecutaron Random Search y DQL; la muestra inicial no permite concluir cuál rinde mejor. Véase [la prueba](../pruebas/primera_baseline.md).

## Random Search

`RandomPolicy`, en `cyberbattle/agents/baseline/learner.py`, explora con `sample_valid_action()`: selecciona acciones aleatorias que pasan la máscara. El ejecutable `baseline/run.py` usa `epsilon=1.0` para esta referencia. No aprende una política; sirve para medir qué se consigue con exploración aleatoria bajo las mismas condiciones. La validez no garantiza éxito, como se explica en [Acciones](acciones.md).

## DQL

```text
observation → características → DQL → acción abstracta → acción Gym
                                                       ↓
                                                 env.step()
                                                       ↓
                                              reward + observation
```

`DeepQLearnerPolicy`, en `cyberbattle/agents/baseline/agent_dql.py`, emplea una red neuronal para estimar valores de acciones a partir de características derivadas de la observación y del historial mantenido por `AgentWrapper`. No introduce la observación completa sin transformar en la red. La acción abstracta se especializa en una acción Gym concreta y se valida; si no puede realizarse, el bucle recurre a exploración.

Durante entrenamiento, la estrategia epsilon-greedy alterna exploración y explotación. Las transiciones de estado, acción, recompensa y estado siguiente se guardan en memoria de experiencia y se muestrean para actualizar la red y aprender una política. La recompensa es la que devuelve el entorno, con las particularidades descritas en [Recompensas y objetivos](recompensas-objetivos.md).
