Objetivo del `event_worker`:

- Consumir eventos desde una cola compartida.
- Procesar un evento a la vez (secuencial).
- Delegar cada tipo de evento a su handler correspondiente.

Nota:

- El worker ahora enruta dos tipos de eventos:
  - `PriceUpdate(new_best_bid)`
  - `ExecutionReportUpdate(execution_report)`

------------------------------------------------------

Importaciones:

* analyze_price_update(new_best_bid)              from price/price_analyzer.rs
* process_execution_report(execution_report)       from orders/requeue/requeue.rs
* event_queue (cola desde donde se consumen eventos)

------------------------------------------------------

Definición de eventos (actual):

    enum Event:
        PriceUpdate(new_best_bid)
        ExecutionReportUpdate(execution_report)

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

                case ExecutionReportUpdate:
                    execution_report = event.execution_report
                    process_execution_report(execution_report)

                default:
                    # Evento no soportado todavía.
                    # Se ignora o se registra para debug.
                    continue

------------------------------------------------------

Notas de diseño:

- El worker centraliza el ruteo de eventos.
- `user_data_stream` ya filtra solo `executionReport`, así que este case recibe payloads relevantes.
- Cada nuevo tipo de evento debe agregar un nuevo `case`.
- `PriceUpdate` mantiene el flujo actual hacia `price_analyzer`.
