*Algoritmo:*

En este archivo se define la función `startUserDataStream(eventQueue)`.
Su objetivo es abrir el User Data Stream de Binance, mantenerlo vivo con reconexión periódica cada 30 minutos, filtrar **solo** eventos `executionReport`, y enviar esos eventos al worker por la cola compartida.

Si llega cualquier otro evento del User Data Stream (por ejemplo account update), se ignora.

Variables de entorno de Binance:
- BINANCE_REST_BASE_URL=https://demo-api.binance.com/api
- BINANCE_WS_BASE_URL=wss://demo-stream.binance.com/ws

-------------------------------------------------------------------------------
* Import createListenKey()                 from streams/binance_user_data_api.rs
* Import keepAliveListenKey(listenKey)     from streams/binance_user_data_api.rs
* Import closeListenKey(listenKey)         from streams/binance_user_data_api.rs
* Import executionReportEvent(payload)     from worker/event_worker.rs
* Import eventQueue                         from worker/event_worker.rs


    function startUserDataStream(eventQueue)

        restBaseUrl = getEnv("BINANCE_REST_BASE_URL")
        wsBaseUrl   = getEnv("BINANCE_WS_BASE_URL")

        while true

            listenKey = createListenKey(restBaseUrl)

            si listenKey == null:
                sleep(5 segundos)
                continuar

            streamUrl = wsBaseUrl + "/" + listenKey
            ws = websocketConnect(streamUrl)

            connectedAt = now()

            while true

                # reconexión forzada cada 30 minutos
                elapsed = now() - connectedAt
                si elapsed >= 30 minutos:
                    break

                rawMessage = ws.readMessage(timeout = 1 segundo)

                # sin mensaje, seguimos el loop para revisar timeout/reconexión
                si rawMessage == null:
                    continue

                parsedMessage = parseJson(rawMessage)

                si parsedMessage es inválido:
                    continue

                eventType = parsedMessage["e"]

                # SOLO filtrar executionReport
                si eventType != "executionReport":
                    continue

                event = executionReportEvent(parsedMessage)
                eventQueue.push(event)

            # al salir del loop interno, renovar stream
            ws.closeSafe()
            closeListenKey(listenKey)

            # pausa breve para evitar loop agresivo en errores de red
            sleep(500 milisegundos)

-------------------------------------------------------------------------------
Notas de diseño:

- La reconexión cada 30 minutos mantiene estable el canal de user data.
- En cada ciclo se crea un nuevo `listenKey` para abrir un WebSocket limpio.
- `executionReport` se envía completo al worker para que el enrutador tome decisiones.
- Cualquier evento no relacionado con ejecuciones se ignora explícitamente.
- `keepAliveListenKey` puede ejecutarse en un timer adicional si se desea robustez extra,
  pero con reconexión forzada cada 30 minutos ya se cumple el requerimiento.
