Objetivo del módulo `bookticker_stream`:

- Suscribirse al WebSocket `bookticker` del activo definido en `state/asset.rs`.
- Escuchar eventos continuamente.
- Filtrar/limpiar el payload y extraer solo `best_bid`.
- Crear `PriceUpdate(new_best_bid)`.
- Enviar el evento al worker por la cola.

------------------------------------------------------

Importaciones:

* get_asset_symbol() se ubica en state/asset.rs
* PriceUpdate(new_best_bid) se ubica en worker/event_worker.rs (tipo de evento)
* event_queue (cola compartida con el worker)

------------------------------------------------------

    function start_bookticker_stream(event_queue):

        symbol = get_asset_symbol()
        stream_url = build_bookticker_ws_url(symbol)

        ws = websocket_connect(stream_url)

        while true:
            raw_message = ws.read_message()

            if raw_message == null:
                continue

            parsed_message = parse_json(raw_message)

            if parsed_message is invalid:
                continue

            new_best_bid = extract_best_bid(parsed_message)

            if new_best_bid == null:
                continue

            event = PriceUpdate(new_best_bid)

            event_queue.push(event)

------------------------------------------------------

Notas de comportamiento:

- El stream no decide si el precio sube o baja; solo publica `PriceUpdate`.
- La lógica de análisis ocurre en el worker/analyzer.
