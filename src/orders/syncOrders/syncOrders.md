*Algoritmo:*

syncOrders es el orquestador.
Solo llama funciones atómicas para traer datos y preparar los datos de sincronización.
Primero consulta order book spot (10 bids y 10 asks), luego calcula en qué precios deben estar las órdenes de compra.
Después valida si debe continuar o terminar el flujo.


-------------------------------------------------------------------------------
* Import getOrderBookSpot()        from   spot/api/get_orderbook.rs
* Import findBuyLevels()           from   orders/syncOrders/buy_levels.rs
* Import shouldContinueSync()      from   orders/syncOrders/should_continue.rs


    function syncOrders()

        orderBook = getOrderBookSpot()

        bestBids = orderBook.bestBids

        targetBuyPrices = findBuyLevels(bestBids)

        shouldContinue = shouldContinueSync(targetBuyPrices)

        if shouldContinue == false:
            return

        // continua con los siguientes pasos de sync

-------------------------------------------------------------------------------
