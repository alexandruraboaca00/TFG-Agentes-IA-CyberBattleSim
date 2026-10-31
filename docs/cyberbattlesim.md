# CyberBattleSim

CyberBattleSim simula ataques sobre redes y máquinas abstractas. En este TFG proporciona un entorno controlado para estudiar y comparar agentes de inteligencia artificial, sin actuar sobre redes reales.

Gymnasium ofrece la interfaz común entre el agente y el entorno: inicia episodios, recibe acciones y devuelve observaciones, recompensas y señales de finalización. No es un algoritmo de aprendizaje.

```text
Agente
   ↓ action
Gymnasium
   ↓
CyberBattleEnv
   ↓
CyberBattleSim
   ↓
observation + reward
   ↓
Agente
```

El agente decide a partir de lo que ha observado; el entorno ejecuta la acción en la simulación y devuelve el resultado. El ciclo se repite hasta que termina el episodio o se alcanza un límite externo.

## Documentación

- [Arquitectura](cyberbattlesim/arquitectura.md)
- [Gymnasium](cyberbattlesim/gymnasium.md)
- [Entorno Chain](cyberbattlesim/entorno-chain.md)
- [Observaciones](cyberbattlesim/observaciones.md)
- [Acciones](cyberbattlesim/acciones.md)
- [Recompensas y objetivos](cyberbattlesim/recompensas-objetivos.md)
- [Agentes baseline](cyberbattlesim/agentes-baseline.md)
- [Integración con IA](cyberbattlesim/integracion-ia.md)
