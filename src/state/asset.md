Objetivo del `asset state`:

- Definir la criptomoneda/par a operar.
- Exponer una forma consistente de leer el símbolo desde otros módulos.
- Permitir cambios controlados del activo configurado.

------------------------------------------------------

Pseudocódigo (`asset`):

    state AssetState:
        asset_symbol = "BTCUSDT"   # valor por defecto

    function get_asset_symbol():
        return asset_symbol

    function set_asset_symbol(new_symbol):
        if new_symbol == null:
            return

        if new_symbol == "":
            return

        # Normalizar para mantener formato consistente
        asset_symbol = to_uppercase(trim(new_symbol))

------------------------------------------------------

Reglas de uso:

- `bookticker_stream` debe usar `get_asset_symbol()` para construir la URL del WebSocket.
- No se debe hardcodear el símbolo en el stream ni en el worker.
