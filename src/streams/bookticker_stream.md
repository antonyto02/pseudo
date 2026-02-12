*Algoritmo:*

En este archivo se define la función llamada start_bookticker_stream() la cual recibe como parámetro una variable llamada event_queue. El propósito de esta función es suscribirse al WebSocket bookticker del activo configurado, escuchar mensajes en bucle continuo, limpiar y validar el payload entrante para extraer únicamente el new_best_bid, y construir el evento PriceUpdate(new_best_bid) para enviarlo a la cola compartida del worker. Si el mensaje llega vacío, inválido o sin best bid, se ignora y el stream continúa sin interrumpirse.


-------------------------------------------------------------------------------
* Import get_asset_symbol()        from   state/asset.rs
* Import PriceUpdate()             from   worker/event_worker.rs
* Import event_queue               from   worker/event_worker.rs



    function start_bookticker_stream(event_queue)


        symbol = get_asset_symbol()
        stream_url = build_bookticker_ws_url(symbol)

        ws = websocket_connect(stream_url)

        while true
                raw_message = ws.read_message()

                Si raw_message == null, continuar

                parsed_message = parse_json(raw_message)

                Si parsed_message es inválido, continuar

                new_best_bid = extract_best_bid(parsed_message)

                Si new_best_bid == null, continuar

                event = PriceUpdate(new_best_bid)

                event_queue.push(event)

-------------------------------------------------------------------------------
