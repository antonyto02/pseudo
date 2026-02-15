*Algoritmo:*

En este archivo se define la función llamada startBooktickerStream() la cual recibe como parámetro una variable llamada eventQueue. El propósito de esta función es suscribirse al WebSocket bookticker de Binance para el activo configurado, leyendo la URL base desde variables de entorno. Para spot se usa BINANCE_WS_BASE_URL y para futures se usa BINANCE_FUTURES_WS_BASE_URL. Luego escucha mensajes en bucle continuo, limpia y valida el payload entrante para extraer únicamente el newBestBid, lo convierte inmediatamente de string a ticks enteros, y construye el evento priceUpdate(newBestBidTicks) para enviarlo a la cola compartida del worker. Si el mensaje llega vacío, inválido o sin best bid, se ignora y el stream continúa sin interrumpirse.

Variables de entorno de Binance:
- BINANCE_REST_BASE_URL=https://demo-api.binance.com/api
- BINANCE_WS_BASE_URL=wss://demo-stream.binance.com/ws
- BINANCE_FUTURES_REST_BASE_URL=https://demo-fapi.binance.com
- BINANCE_FUTURES_WS_BASE_URL=wss://demo-fstream.binance.com



-------------------------------------------------------------------------------
* Import getAssetSymbol()          from   state/asset.rs
* Import priceUpdate()             from   worker/event_worker.rs
* Import eventQueue                from   worker/event_worker.rs
* Import priceStringToTicks()      from   price/conversion/string_to_ticks.rs



    function startBooktickerStream(eventQueue)


        symbol = getAssetSymbol()

        wsBaseUrl = getEnv("BINANCE_WS_BASE_URL")
        futuresWsBaseUrl = getEnv("BINANCE_FUTURES_WS_BASE_URL")

        baseUrl = selectWsBaseUrl(wsBaseUrl, futuresWsBaseUrl)
        streamUrl = buildBooktickerWsUrl(baseUrl, symbol)

        ws = websocketConnect(streamUrl)

        while true
                rawMessage = ws.readMessage()

                Si rawMessage == null, continuar

                parsedMessage = parseJson(rawMessage)

                Si parsedMessage es inválido, continuar

                newBestBid = extractBestBid(parsedMessage)

                Si newBestBid == null, continuar

                newBestBidTicks = priceStringToTicks(newBestBid)

                event = priceUpdate(newBestBidTicks)

                eventQueue.push(event)

-------------------------------------------------------------------------------
