*Algoritmo:*

Esta función valida si syncOrders debe continuar o terminar.
Compara el bid más alto de las órdenes en RAM contra el bid más alto de los precios objetivo.
Trabaja en ticks enteros para evitar mezclar strings con enteros.

Reglas de seguridad:
1) No inicializar con orderBlocks[0].bid_price.
2) Ignorar bid_price null o vacío.
3) Si no hay bids válidos en RAM, retornar true.
4) Si targetBuyPrices está vacío, retornar false.
5) Comparar solo enteros (ticks).


-------------------------------------------------------------------------------
* Import getOrderBlocks()              from   state/orders.rs
* Import priceStringToTicks()          from   price/conversion/string_to_ticks.rs


    function shouldContinueSync(targetBuyPrices)

        if length(targetBuyPrices) == 0:
            return false

        orderBlocks = getOrderBlocks()

        highestOrderBidTicks = null

        for each block in orderBlocks:

            if block.bid_price == null:
                continue

            if block.bid_price == "":
                continue

            bidTicks = priceStringToTicks(block.bid_price)

            if highestOrderBidTicks == null:
                highestOrderBidTicks = bidTicks
                continue

            if bidTicks > highestOrderBidTicks:
                highestOrderBidTicks = bidTicks

        if highestOrderBidTicks == null:
            return true

        highestTargetBid = targetBuyPrices[0]

        for each price in targetBuyPrices:
            if price > highestTargetBid:
                highestTargetBid = price

        if highestOrderBidTicks >= highestTargetBid:
            return false

        return true

-------------------------------------------------------------------------------
