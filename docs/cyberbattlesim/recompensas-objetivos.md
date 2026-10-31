# Recompensas y objetivos

## Cálculo de recompensa

`AgentActions` en `cyberbattle/simulation/actions.py` devuelve un `ActionResult` con resultado y recompensa interna. Un exploit procesado puede sumar 7 por primer ataque, 5 por nodo nuevo, 3 por credencial nueva y 2 por propiedad nueva; resta el coste de la vulnerabilidad. También existen penalizaciones por repetición y fallos. El valor de un nodo se obtiene al adquirirlo por primera vez, no por descubrirlo. Una conexión nueva con éxito entrega su valor; repetirla produce una penalización interna.

El coste no siempre llega como recompensa negativa al agente. En `CyberBattleEnv.step()`:

1. Se calcula el resultado de la acción y se comprueban los objetivos.
2. Si gana el atacante o se incumple una restricción del defensor, la recompensa de esa transición se **sustituye** por `winning_reward` (por defecto 5000).
3. Si gana el defensor, se sustituye por `losing_reward` (por defecto 0).
4. En otro caso se aplica `reward = max(0.0, reward)`.

Así, un resultado interno negativo en una transición no terminal llega como 0. Los valores terminales configurables se devuelven sin ese recorte. La lista de recompensas del episodio se actualiza después de comprobar el objetivo: el umbral acumulado se evalúa con recompensas de pasos anteriores, no con la recompensa de la acción actual.

## `AttackerGoal`

En `cyberbattle/_env/cyberbattle_env.py`, sus condiciones se combinan:

| Parámetro | Condición |
|---|---|
| `reward` | Recompensa acumulada al menos igual al umbral. |
| `own_atleast` | Mínimo de nodos controlados. |
| `own_atleast_percent` | Fracción mínima de todos los nodos controlados. |
| `low_availability` | Con defensor activo, disponibilidad estrictamente inferior al umbral. |

`AttackerGoal(own_atleast_percent=1.0)` exige controlar todos los nodos, incluido el inicial, además de satisfacer los demás valores por defecto. Sin defensor activo se omite la condición de disponibilidad.

## Defensor y finalización

`DefenderGoal(eviction=True)` se cumple cuando el atacante no controla ningún nodo. `DefenderConstraint(maintain_sla=...)` exige mantener la disponibilidad; con defensor activo, caer por debajo del umbral rompe la restricción y provoca victoria del atacante. La disponibilidad representa el estado de servicio de la red, no el porcentaje de nodos controlados.

`terminated` corresponde a estas condiciones terminales. `step()` devuelve `self.__done, False` como `terminated, truncated`: el entorno base no usa `truncated` para sus condiciones internas. Un wrapper o el bucle de experimentación puede limitar la duración por separado; ese límite debe registrarse al comparar episodios.
