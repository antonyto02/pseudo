*Algoritmo:*

En este archivo se define la función `startUserDataStream(eventQueue)`. Su responsabilidad es abrir el User Data Stream de Binance, mantener la conexión activa con recambio/reconexión cada 30 minutos, filtrar únicamente eventos `executionReport`, mapearlos a un evento interno y enviarlos al worker por la cola compartida.

Reglas clave:
- Solo se procesan mensajes con `e == "executionReport"`.
- Cualquier otro tipo de evento del User Data Stream se ignora.
- Cada 30 minutos se debe renovar el ciclo de conexión (keepalive + reconexión controlada).

Variables de entorno sugeridas:
- BINANCE_REST_BASE_URL=https://demo-api.binance.com/api
- BINANCE_WS_BASE_URL=wss://demo-stream.binance.com/ws
- BINANCE_API_KEY=xxx

-------------------------------------------------------------------------------
* Import executionReportUpdate()      from   worker/event_worker.rs
* Import eventQueue                   from   worker/event_worker.rs


    function startUserDataStream(eventQueue)

        restBaseUrl = getEnv("BINANCE_REST_BASE_URL")
        wsBaseUrl   = getEnv("BINANCE_WS_BASE_URL")
        apiKey      = getEnv("BINANCE_API_KEY")

        while true

            # 1) Crear listenKey para iniciar un nuevo ciclo de 30 minutos
            listenKey = createListenKey(restBaseUrl, apiKey)

            if listenKey == null
                sleep(3 seconds)
                continue

            streamUrl = wsBaseUrl + "/" + listenKey
            ws = websocketConnect(streamUrl)

            if ws == null
                closeListenKey(restBaseUrl, apiKey, listenKey)
                sleep(3 seconds)
                continue

            cycleStartedAt = now()

            # 2) Leer mensajes hasta cumplir 30 minutos o hasta error
            while true

                elapsed = now() - cycleStartedAt

                if elapsed >= 30 minutes
                    # rotación preventiva de conexión
                    break

                rawMessage = ws.readMessage(timeout = 1 second)

                if rawMessage == null
                    # timeout o heartbeat sin payload
                    # seguir esperando mensajes
                    continue

                parsedMessage = parseJson(rawMessage)

                if parsedMessage es inválido
                    continue

                eventType = parsedMessage["e"]

                if eventType != "executionReport"
                    # User Data Stream trae varios eventos;
                    # solo nos interesan executionReport
                    continue

                internalExecutionReport = mapExecutionReport(parsedMessage)

                if internalExecutionReport == null
                    continue

                event = executionReportUpdate(internalExecutionReport)
                eventQueue.push(event)

            # 3) Fin del ciclo de 30 min: keepalive + cierre + reconexión
            pingListenKeyKeepAlive(restBaseUrl, apiKey, listenKey)
            ws.close()
            closeListenKey(restBaseUrl, apiKey, listenKey)

            # reconexión inmediata al siguiente ciclo (con pequeño backoff opcional)
            sleep(250 milliseconds)

-------------------------------------------------------------------------------

Notas de diseño:
- La rotación cada 30 minutos reduce riesgo de streams stale.
- Si Binance responde error temporal, se reintenta en bucle con backoff corto.
- El worker recibe eventos ya filtrados, por lo que el ruteo downstream es más simple.
