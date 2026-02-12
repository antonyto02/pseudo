*Algoritmo:*

En este archivo se define la función llamada startBooktickerStream() la cual recibe como parámetro una variable llamada eventQueue. El propósito de esta función es suscribirse al WebSocket bookticker del activo configurado, escuchar mensajes en bucle continuo, limpiar y validar el payload entrante para extraer únicamente el newBestBid, y construir el evento priceUpdate(newBestBid) para enviarlo a la cola compartida del worker. Si el mensaje llega vacío, inválido o sin best bid, se ignora y el stream continúa sin interrumpirse.


-------------------------------------------------------------------------------
* Import getAssetSymbol()          from   state/asset.rs
* Import priceUpdate()             from   worker/event_worker.rs
* Import eventQueue                from   worker/event_worker.rs



    function startBooktickerStream(eventQueue)


        symbol = getAssetSymbol()
        streamUrl = buildBooktickerWsUrl(symbol)

        ws = websocketConnect(streamUrl)

        while true
                rawMessage = ws.readMessage()

                Si rawMessage == null, continuar

                parsedMessage = parseJson(rawMessage)

                Si parsedMessage es inválido, continuar

                newBestBid = extractBestBid(parsedMessage)

                Si newBestBid == null, continuar

                event = priceUpdate(newBestBid)

                eventQueue.push(event)

-------------------------------------------------------------------------------
