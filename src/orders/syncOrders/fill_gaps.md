*Algoritmo:*

Esta función rellena huecos en órdenes buy.
Recorre targetBuyPrices y, cuando falta un precio en RAM, crea la orden en Binance Spot.
Si Binance responde ok, calcula sellPrice con orderbook y asigna la compra al primer bloque null.
No retorna nada.


-------------------------------------------------------------------------------
* Import getOrderBlocks()              from   state/orders.rs
* Import getAsset()                    from   state/asset.rs
* Import placeSpotLimitBuy()           from   spot/api/place_limit_buy.rs
* Import findSellPriceFromBook()       from   orders/syncOrders/sell_price_from_book.rs
* Import assignBuyToFirstEmptyBlock()  from   orders/syncOrders/assign_buy_block.rs


    function fillMissingBuyOrders(targetBuyPrices, bestAsks)

        asset = getAsset()
        orderBlocks = getOrderBlocks()

        qty = asset.qtyPerBlock
        sellTickOffset = asset.sellTickOffset

        for each targetPrice in targetBuyPrices:

            existsInRam = false

            for each block in orderBlocks:
                if block.bid_price == targetPrice:
                    existsInRam = true
                    break

            if existsInRam == true:
                continue

            buyResult = placeSpotLimitBuy(targetPrice, qty)

            if buyResult.ok == false:
                continue

            sellPrice = findSellPriceFromBook(targetPrice, sellTickOffset, bestAsks)

            assignBuyToFirstEmptyBlock(targetPrice, sellPrice, buyResult.orderId)

-------------------------------------------------------------------------------
