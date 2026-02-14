*Algoritmo:*

syncOrders es el orquestador.
Solo llama funciones atómicas para traer datos y sincronizar RAM.
Primero consulta order book spot (10 bids y 10 asks), luego actualiza bloques en memoria.


-------------------------------------------------------------------------------
* Import getOrderBookSpot()        from   spot/api/get_orderbook.rs
* Import applyOrderBookSnapshot()  from   state/orders.rs


    function syncOrders()

        orderBook = getOrderBookSpot()

        bestBids = orderBook.bestBids
        bestAsks = orderBook.bestAsks

        applyOrderBookSnapshot(bestBids, bestAsks)

-------------------------------------------------------------------------------
