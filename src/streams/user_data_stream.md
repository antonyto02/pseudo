*Algoritmo:*

En este archivo se define la función llamada startUserDataStream() la cual recibe como parámetro una variable llamada eventQueue. El propósito de esta función es conectarse al User Data Stream de Binance, mantener viva la sesión (listen key), forzar una reconexión controlada cada 30 minutos y filtrar únicamente eventos executionReport. Cada executionReport válido se transforma a un evento executionReport(payload) y se envía a la cola compartida del worker para que sea procesado de forma secuencial.

Variables de entorno de Binance:
- BINANCE_API_KEY=
- BINANCE_API_SECRET=
- BINANCE_REST_BASE_URL=https://demo-api.binance.com/api
- BINANCE_WS_BASE_URL=wss://demo-stream.binance.com/ws
- BINANCE_FUTURES_REST_BASE_URL=https://demo-fapi.binance.com
- BINANCE_FUTURES_WS_BASE_URL=wss://demo-fstream.binance.com
- USER_STREAM_RECONNECT_MINUTES=30


-------------------------------------------------------------------------------
* Import executionReport()         from   worker/event_worker.rs
* Import eventQueue                from   worker/event_worker.rs



    function startUserDataStream(eventQueue)

        reconnectMinutes = toInt(getEnvOrDefault("USER_STREAM_RECONNECT_MINUTES", "30"))

        while true

            restBaseUrl = selectRestBaseUrl()
            wsBaseUrl = selectWsBaseUrl()

            listenKey = createListenKey(restBaseUrl)

            si listenKey == null
                sleep(2 seconds)
                continue

            streamUrl = buildUserDataWsUrl(wsBaseUrl, listenKey)

            ws = websocketConnect(streamUrl)

            si ws == null
                sleep(2 seconds)
                continue

            startedAt = nowUtcMs()
            nextKeepAliveAt = startedAt + (25 minutes)
            reconnectAt = startedAt + (reconnectMinutes minutes)

            while true

                now = nowUtcMs()

                si now >= reconnectAt
                    ws.close()
                    break

                si now >= nextKeepAliveAt
                    keepAliveOk = keepAliveListenKey(restBaseUrl, listenKey)
                    nextKeepAliveAt = now + (25 minutes)

                    si keepAliveOk == false
                        ws.close()
                        break

                rawMessage = ws.readMessage(timeout=1 second)

                si rawMessage == null
                    continue

                parsedMessage = parseJson(rawMessage)

                si parsedMessage es inválido
                    continue

                eventType = parsedMessage["e"]

                si eventType != "executionReport"
                    continue

                payload = buildExecutionReportPayload(parsedMessage)

                si payload == null
                    continue

                event = executionReport(payload)
                eventQueue.push(event)

            safeClose(ws)

-------------------------------------------------------------------------------
Notas de diseño:

- El stream no aplica lógica de negocio de fills/cancels; solo filtra y enruta.
- El worker sigue siendo el serializador principal (un evento a la vez).
- La reconexión forzada cada 30 min reduce sesiones largas y facilita recuperación ante fallos silenciosos.
- Si falla creación/keepalive del listen key, el algoritmo reintenta sin tumbar el proceso.
