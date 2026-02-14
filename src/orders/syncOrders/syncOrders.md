*Algoritmo:*

syncOrders es el orquestador.
Solo llama funciones atómicas para traer datos y preparar los datos de sincronización.
Primero consulta order book spot (10 bids y 10 asks), luego calcula en qué precios deben estar las órdenes de compra.


-------------------------------------------------------------------------------
* Import getOrderBookSpot()        from   spot/api/get_orderbook.rs
* Import findBuyLevels()           from   orders/syncOrders/buy_levels.rs


    function syncOrders()

        orderBook = getOrderBookSpot()

        bestBids = orderBook.bestBids
        bestAsks = orderBook.bestAsks

        targetBuyPrices = findBuyLevels(bestBids)

-------------------------------------------------------------------------------
