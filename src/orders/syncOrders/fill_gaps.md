*Algoritmo:*

Esta función rellena huecos en órdenes buy.
Recorre targetBuyPrices y, cuando falta un precio en RAM, crea la orden en Binance Spot.
Si Binance responde ok, calcula sellPrice con orderbook y asigna la compra al primer bloque null.
Para validar existencia en RAM, convierte bid_price (string) a ticks antes de comparar.
Antes de enviar a Binance y guardar en RAM, convierte los ticks a string de precio.
No retorna nada.


-------------------------------------------------------------------------------
* Import getOrderBlocks()              from   state/orders.rs
* Import getAsset()                    from   state/asset.rs
* Import placeSpotLimitBuy()           from   spot/api/place_limit_buy.rs
* Import findSellPriceFromBook()       from   orders/syncOrders/sell_price_from_book.rs
* Import assignBuyToFirstEmptyBlock()  from   orders/syncOrders/assign_buy_block.rs
* Import priceStringToTicks()          from   price/conversion/string_to_ticks.rs
* Import ticksToPriceString()          from   price/conversion/ticks_to_string.rs


    function fillMissingBuyOrders(targetBuyPrices, bestAsks)

        asset = getAsset()
        orderBlocks = getOrderBlocks()

        qty = asset.qtyPerBlock
        sellTickOffset = asset.sellTickOffset

        for each targetPrice in targetBuyPrices:

            existsInRam = false

            for each block in orderBlocks:

                if block.bid_price == null:
                    continue

                if block.bid_price == "":
                    continue

                bidTicks = priceStringToTicks(block.bid_price)

                if bidTicks == targetPrice:
                    existsInRam = true
                    break

            if existsInRam == true:
                continue

            targetPriceStr = ticksToPriceString(targetPrice)

            buyResult = placeSpotLimitBuy(targetPriceStr, qty)

            if buyResult.ok == false:
                continue

            sellPrice = findSellPriceFromBook(targetPrice, sellTickOffset, bestAsks)
            sellPriceStr = ticksToPriceString(sellPrice)

            assignBuyToFirstEmptyBlock(targetPriceStr, sellPriceStr, buyResult.orderId)

-------------------------------------------------------------------------------
