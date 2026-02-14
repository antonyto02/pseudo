*Algoritmo:*

syncOrders es el orquestador.
Solo llama funciones atómicas para traer datos y preparar los datos de sincronización.
Primero consulta order book spot (10 bids y 10 asks), luego calcula en qué precios deben estar las órdenes de compra.
Después valida si debe continuar o terminar el flujo.
Si continúa, limpia/cancela las órdenes fuera de rango y luego rellena huecos.


-------------------------------------------------------------------------------
* Import getOrderBookSpot()            from   spot/api/get_orderbook.rs
* Import findBuyLevels()               from   orders/syncOrders/buy_levels.rs
* Import shouldContinueSync()          from   orders/syncOrders/should_continue.rs
* Import cleanOutOfRangeBuyOrders()    from   orders/syncOrders/clean_out_of_range.rs
* Import fillMissingBuyOrders()        from   orders/syncOrders/fill_gaps.rs


    function syncOrders()

        orderBook = getOrderBookSpot()

        bestBids = orderBook.bestBids

        targetBuyPrices = findBuyLevels(bestBids)

        shouldContinue = shouldContinueSync(targetBuyPrices)

        if shouldContinue == false:
            return

        cleanOutOfRangeBuyOrders(targetBuyPrices)
        fillMissingBuyOrders(targetBuyPrices)

        // continua con los siguientes pasos de sync

-------------------------------------------------------------------------------
