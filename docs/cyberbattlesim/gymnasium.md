# Gymnasium y CyberBattleSim

**Gymnasium no es un algoritmo de IA ni de aprendizaje por refuerzo**. Es una interfaz entre agente y simulación: define cómo iniciar un episodio, enviar acciones y recibir resultados. El aprendizaje, si existe, pertenece al agente.

`CyberBattleEnv` hereda de `gymnasium.Env` a través de `CyberBattleSpaceKind`, en `cyberbattle/_env/cyberbattle_env.py`.

```python
obs, info = env.reset()
obs, reward, terminated, truncated, info = env.step(action)
```

| Elemento | Función |
|---|---|
| `observation_space` | Describe la estructura y límites de las observaciones. |
| `action_space` | Describe las formas y rangos de acciones; no garantiza su éxito. |
| `reset()` | Inicia un episodio y devuelve observación inicial e información. |
| `step(action)` | Ejecuta una acción y devuelve observación, recompensa y señales de fin. |
| `reward` | Señal numérica de la transición. |
| `terminated` | Indica una condición terminal del entorno. |
| `truncated` | Indica un corte externo, por ejemplo un límite de tiempo. |
| `info` | Datos auxiliares, no necesariamente parte del estado usado para decidir. |

## Dentro de `reset()`

El entorno copia el escenario inicial, reinicia descubrimientos, credenciales, recompensas y contador, y crea los actuadores. Identifica los nodos con agente instalado, inicializa el generador con la semilla y construye observación, máscara e `info`. No ejecuta una acción.

## Dentro de `step()`

Traduce la acción a identificadores internos y la ejecuta mediante `AgentActions`. Convierte el resultado en observación y recompensa, actualiza el defensor si existe, comprueba objetivos y devuelve la transición. `info` incluye contador, disponibilidad, duración y caché interna de credenciales. El campo `duration_in_ms` se calcula con `time.time()` en segundos pese a su nombre.

El entorno base devuelve siempre `False` para `truncated`; un wrapper puede añadir un límite externo. Los detalles están en [Recompensas y objetivos](recompensas-objetivos.md), [Observaciones](observaciones.md) y [Acciones](acciones.md).
