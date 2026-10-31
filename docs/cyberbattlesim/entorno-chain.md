# Entorno Chain

`CyberBattleChain-v0` está registrado en `cyberbattle/__init__.py`. `CyberBattleChain`, en `cyberbattle/_env/cyberbattle_chain.py`, crea el escenario mediante `new_environment(size)` de `cyberbattle/samples/chainpattern/chainpattern.py`.

```text
START → Linux → Windows → Linux → Windows → ... → Linux final / FLAG
```

`size` debe ser par y cuenta los nodos intermedios; hay `size + 2` nodos en total, incluidos `start` y el final. `start` comienza controlado, sin servicios y con valor 0. Los nodos intermedios tienen valor 100; el Linux final tiene valor 1000 y la propiedad `FLAG:Linux`. El objetivo configurado del episodio determina la victoria: la etiqueta FLAG por sí sola no es una condición terminal especial.

## Patrón del escenario

Los Linux intermedios ofrecen HTTPS y SSH; los Windows, HTTPS y RDP. El nodo final ofrece HTTPS y SSH. Cada servicio SSH o RDP relevante acepta la contraseña generada para ese nodo. La conexión exitosa permite movimiento lateral y control.

Las acciones locales útiles siguen el patrón:

| Nodo controlado | Descubrimiento de máquina | Credencial siguiente |
|---|---|---|
| `start` | La filtración de credencial descubre el primer Linux | `ScanExplorerRecentFiles`: SSH |
| Linux intermedio | `ScanBashHistory`: siguiente Windows | `CrackKeepPassX`: RDP |
| Windows intermedio | `ScanExplorerRecentFiles`: siguiente Linux | `CrackKeepPass`: SSH |

Una filtración de credenciales también descubre su nodo destino; no es obligatorio descubrirlo antes por la acción de exploración. Las acciones locales inadecuadas para el sistema son trampas con resultado fallido y costes altos. Las vulnerabilidades remotas `ProbeLinux` y `ProbeWindows` permiten intentar identificar propiedades; su resultado depende del nodo. No conceden por sí mismas control.

Ejemplo: `start` → filtrar credencial SSH y descubrir Linux → conectar → filtrar credencial RDP y descubrir Windows → conectar → filtrar la siguiente credencial SSH → continuar hasta el Linux final. También puede usarse la acción de descubrimiento de nodos correspondiente antes de filtrar la credencial.

El comentario inicial de `chainpattern.py` afirma que `start` filtra credenciales para todos los nodos, pero la implementación solo filtra la del primer Linux.
