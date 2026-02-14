*Algoritmo:*

Esta función limpia órdenes de compra que quedaron fuera de targetBuyPrices.
Recorre bloque por bloque.
Si bid_price es null, ignora.
Si bid_price existe en targetBuyPrices, ignora.
Si bid_price no existe en targetBuyPrices, cancela en Binance y luego limpia el bloque en RAM.
No retorna nada.


-------------------------------------------------------------------------------
* Import getOrderBlocks()              from   state/orders.rs
* Import resetOrderBlock()             from   state/orders.rs
* Import cancelSpotBuyOrders()         from   spot/api/cancel_buy_orders.rs


    function cleanOutOfRangeBuyOrders(targetBuyPrices)

        orderBlocks = getOrderBlocks()

        for i desde 0 hasta length(orderBlocks) - 1:

            block = orderBlocks[i]

            if block.bid_price == null:
                continue

            if targetBuyPrices includes block.bid_price:
                continue

            cancelOk = cancelSpotBuyOrders(block.buy_order_ids)

            if cancelOk == true:
                resetOrderBlock(i)

-------------------------------------------------------------------------------
