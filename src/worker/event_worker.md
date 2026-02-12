Objetivo del `event_worker`:

- Consumir eventos desde una cola compartida.
- Procesar un evento a la vez (secuencial).
- Delegar cada tipo de evento a su handler correspondiente.

Nota:

- Por ahora solo existe `PriceUpdate(new_best_bid)`.
- Más adelante se agregarán otros tipos de eventos.

------------------------------------------------------

Importaciones:

* analyze_price_update(new_best_bid) se ubica en price/price_analyzer.rs
* event_queue (cola desde donde se consumen eventos)

------------------------------------------------------

Definición de eventos (actual):

    enum Event:
        PriceUpdate(new_best_bid)

    # Futuro:
    # EventType2(...)
    # EventType3(...)

------------------------------------------------------

    function start_event_worker(event_queue):

        while true:
            event = event_queue.pop_blocking()

            if event == null:
                continue

            switch event.type:

                case PriceUpdate:
                    new_best_bid = event.new_best_bid
                    analyze_price_update(new_best_bid)

                default:
                    # Evento no soportado todavía.
                    # Se ignora o se registra para debug.
                    continue

------------------------------------------------------

Notas de diseño:

- El worker centraliza el ruteo de eventos.
- Cada nuevo tipo de evento debe agregar un nuevo `case`.
- `PriceUpdate` mantiene el flujo actual hacia `price_analyzer`.
