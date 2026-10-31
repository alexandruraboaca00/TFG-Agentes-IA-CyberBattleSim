# Acciones

`CyberBattleEnv` acepta un diccionario con exactamente una de las tres claves. Los índices deben pertenecer a los rangos del entorno.

```python
{"local_vulnerability": [source_node, vulnerability]}
{"remote_vulnerability": [source_node, target_node, vulnerability]}
{"connect": [source_node, target_node, port, credential]}
```

- `source_node`: índice externo de un nodo descubierto y controlado desde el que se actúa.
- `target_node`: índice externo de un nodo descubierto al que se dirige el intento.
- `vulnerability`: índice en la lista de identificadores locales o remotos correspondiente. Una acción local se aplica al origen; una remota, al destino.
- `port`: índice en la lista de puertos del escenario.
- `credential`: posición de la credencial en la caché, no la contraseña.

En `cyberbattle/_env/cyberbattle_env.py`, `__execute_action()` traduce índices de nodos mediante la lista de descubrimiento y los demás mediante `identifiers` o la caché. Por ejemplo, el índice externo 1 no es necesariamente un ID interno llamado `1`; `_discovered_nodes[1]` proporciona ese ID. El actuador comprueba las condiciones efectivas y ejecuta el intento.

## Action mask

`compute_action_mask()` crea matrices de ceros y `__update_action_mask()` marca con 1 las combinaciones habilitadas: 0 significa que la máscara no permite el intento; 1 que lo permite. Para nodos de origen controlados:

- Local: marca vulnerabilidades presentes en el nodo o la biblioteca global.
- Remota: marca todas las vulnerabilidades remotas para cada destino descubierto.
- Conexión: marca todos los puertos y credenciales ya almacenadas para cada destino descubierto.

La máscara no comprueba que la credencial corresponda al destino, que el puerto esté abierto o que una vulnerabilidad remota funcione. **Una acción válida no tiene por qué ser buena ni exitosa.**

`is_action_valid()` comprueba rangos y máscara. `sample_valid_action()` toma muestras hasta encontrar una permitida; no selecciona necesariamente una acción útil. Son herramientas de selección, no sustituyen las comprobaciones del actuador. En particular, `step()` ejecuta directamente la acción y no invoca `is_action_valid()` como filtro general: una acción mal formada puede producir una excepción.
