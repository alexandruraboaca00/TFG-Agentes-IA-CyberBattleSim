# Arquitectura de CyberBattleSim

```text
Escenario / red
       ↓
CyberBattleEnv
       ↓
Gymnasium
       ↓
Agente
```

La simulación representa una red abstracta de máquinas y las posibilidades de un atacante que empieza con acceso a alguna de ellas. No ejecuta los ataques reales que sus acciones representan.

## Modelo de red

En `cyberbattle/simulation/model.py`, `Environment` contiene la red y sus identificadores; `NodeInfo` describe cada nodo. Un nodo representa una máquina con estado, valor y elementos como:

- **Servicios**: puertos o nombres de servicio que escuchan y pueden requerir credenciales (`ListeningService`).
- **Vulnerabilidades**: acciones locales o remotas con precondiciones, coste y resultado (`VulnerabilityInfo`).
- **Credenciales**: secretos asociados a un nodo y puerto que pueden descubrirse y utilizarse para conectar.
- **Propiedades**: etiquetas como sistema operativo o software, utilizadas para describir máquinas y condiciones de ataques.
- **Privilegios**: nivel de acceso del atacante, desde ausencia de acceso hasta niveles como usuario local, administrador o sistema.

Un escenario concreta estas piezas; [Chain](entorno-chain.md) define una red con patrones repetidos.

## Ejecución

`CyberBattleEnv`, en `cyberbattle/_env/cyberbattle_env.py`, adapta el modelo a Gymnasium: mantiene lo descubierto, traduce índices, construye observaciones y gestiona objetivos. La ejecución simulada de exploits y conexiones corresponde a `AgentActions`, en `cyberbattle/simulation/actions.py`; esta clase comprueba condiciones y modifica el estado del modelo. Un actuador separado permite acciones del defensor.

El agente interactúa con la interfaz descrita en [Gymnasium](gymnasium.md), sin necesitar acceso directo al estado completo.
