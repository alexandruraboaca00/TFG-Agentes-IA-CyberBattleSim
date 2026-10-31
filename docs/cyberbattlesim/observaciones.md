# Observaciones

```text
Estado completo de la simulación ≠ Observación del agente
```

La simulación conoce la red completa; la observación recoge lo descubierto y el resultado de la última acción. Su definición y construcción están en `cyberbattle/_env/cyberbattle_env.py`, en `Observation` y `__observation_reward_from_action_result()`.

## Resultado de la última acción

Estos campos se reinician en cada transición; no son un historial acumulado.

| Campo | Significado y uso |
|---|---|
| `newly_discovered_nodes_count` | Número de nodos nuevos descubiertos en esa acción. |
| `lateral_move` | Indicador de resultado de movimiento lateral; puede aparecer también en una conexión repetida. |
| `customer_data_found` | Indica descubrimiento de datos de cliente. |
| `probe_result` | 0 sin sondeo, 1 fallido, 2 exitoso; ayuda a interpretar una prueba remota. |
| `escalation` | Nivel de escalada devuelto, o ausencia de escalada. |
| `leaked_credentials` | Tuplas `[ocupada, índice, nodo, puerto]` para credenciales nuevas; la clave real no se incluye en esta codificación. |

## Estado acumulado

- `discovered_node_count`: número de nodos conocidos, aumenta al descubrirlos.
- `discovered_nodes_properties`: matriz de nodos por propiedades; los unos representan propiedades descubiertas. Puede ayudar a elegir pruebas o exploits.
- `nodes_privilegelevel`: vector de niveles de acceso por índice externo de nodo; permite distinguir nodos controlados.

**Detalle de implementación:** aunque los comentarios describen 2 como desconocido, `__property_vector()` inicializa a 0 también las propiedades no descubiertas de nodos no controlados. El relleno de la matriz también usa ceros. Por tanto, en esta versión un 0 no permite distinguir con seguridad ausencia de propiedad de falta de conocimiento o relleno; usa el número de nodos descubiertos para delimitar filas.

## Credenciales y acciones

`credential_cache_length` indica cuántas entradas son reales. `credential_cache_matrix` contiene para cada una `[índice del nodo destino, índice del puerto]`; la posición de la fila es el índice de credencial para conectar. Las filas sobrantes pueden estar rellenadas con ceros. La caché crece al descubrir credenciales nuevas.

`action_mask` contiene matrices de acciones permitidas según el estado actual; cambia con nodos controlados, descubrimientos y credenciales. Véase [Acciones](acciones.md).

## Información adicional

`_discovered_nodes` relaciona cada índice externo con su identificador interno. `_explored_network` es un grafo NetworkX de nodos conocidos e interacciones, con información interna oculta en nodos no controlados. No son espacios numéricos Gym habituales. Además, `info` incluye `credential_cache` con objetos internos y sus secretos: si se utiliza, conviene distinguir este acceso auxiliar de la codificación de la observación.
