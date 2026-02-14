*Algoritmo:*

Esta función valida si syncOrders debe continuar o terminar.
Compara el bid más alto de las órdenes en RAM contra el bid más alto de los precios objetivo.
Si el bid más alto en RAM ya es igual o mayor al objetivo más alto, syncOrders debe salir.


-------------------------------------------------------------------------------
* Import getOrderBlocks()          from   state/orders.rs


    function shouldContinueSync(targetBuyPrices)

        orderBlocks = getOrderBlocks()

        highestOrderBid = orderBlocks[0].bid_price

        for each block in orderBlocks:
            if block.bid_price > highestOrderBid:
                highestOrderBid = block.bid_price

        highestTargetBid = targetBuyPrices[0]

        for each price in targetBuyPrices:
            if price > highestTargetBid:
                highestTargetBid = price

        if highestOrderBid >= highestTargetBid:
            return false

        return true

-------------------------------------------------------------------------------
