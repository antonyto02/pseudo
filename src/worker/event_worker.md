Objetivo del `event_worker`:

- Consumir eventos desde una cola compartida.
- Procesar un evento a la vez (secuencial).
- Delegar cada tipo de evento a su handler correspondiente.

Nota:

- Actualmente existen `PriceUpdate(new_best_bid)` y `ExecutionReport(payload)`.
- Más adelante se pueden agregar nuevos tipos de eventos.

------------------------------------------------------

Importaciones:

* analyze_price_update(new_best_bid)       se ubica en price/price_analyzer.rs
* handle_execution_report(payload)         se ubica en orders/execution_report_handler.rs
* event_queue (cola desde donde se consumen eventos)

------------------------------------------------------

Definición de eventos (actual):

    enum Event:
        PriceUpdate(new_best_bid)
        ExecutionReport(payload)

    # Futuro:
    # EventType3(...)
    # EventType4(...)

------------------------------------------------------

Constructores helper de evento:

    function priceUpdate(new_best_bid):
        return Event.PriceUpdate(new_best_bid)


    function executionReportEvent(payload):
        return Event.ExecutionReport(payload)

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

                case ExecutionReport:
                    payload = event.payload
                    handle_execution_report(payload)

                default:
                    # Evento no soportado todavía.
                    # Se ignora o se registra para debug.
                    continue

------------------------------------------------------

Notas de diseño:

- El worker centraliza el ruteo de eventos.
- El User Data Stream solo empuja `ExecutionReport`, ya filtrado desde el stream.
- Cada nuevo tipo de evento debe agregar un nuevo `case` y su handler.
- `PriceUpdate` mantiene el flujo actual hacia `price_analyzer`.
