*Algoritmo:*

En este archivo se define la función shortOpenAnalyzer(new_best_bid_ticks).
La función recorre los bloques de órdenes en RAM y abre short cuando:
1) el bloque no tiene short abierto (`has_open_short == "false"`), y
2) `new_best_bid_ticks` es menor al `bid_price` del bloque convertido a ticks.

Si se cumple, calcula la cantidad a abrir en x1 como:
`qtyPerBlock - filled_sell`.
Luego llama a openShort(qty). Si Binance responde ok, marca el bloque con short abierto y guarda el tamaño en `short_size` (string).


-------------------------------------------------------------------------------
* Import getOrderBlocks()        from   state/orders.rs
* Import setOrderBlock()         from   state/orders.rs
* Import getAsset()              from   state/asset.rs
* Import openShort()             from   binance/futures/api/open_short.rs
* Import priceStringToTicks()    from   price/conversion/string_to_ticks.rs


    function shortOpenAnalyzer(new_best_bid_ticks)

        asset = getAsset()
        orderBlocks = getOrderBlocks()

        for i desde 0 hasta length(orderBlocks) - 1:

            block = orderBlocks[i]

            if block.has_open_short != "false":
                continue

            if block.bid_price == null:
                continue

            if block.bid_price == "":
                continue

            bid_price_ticks = priceStringToTicks(block.bid_price)

            if new_best_bid_ticks >= bid_price_ticks:
                continue

            shortQty = toNumber(asset.qtyPerBlock) - toNumber(block.filled_sell)

            if shortQty <= 0:
                continue

            result = openShort(shortQty)

            if result.ok == false:
                continue

            block.has_open_short = "true"
            block.short_size = toString(shortQty)

            setOrderBlock(i, block)

-------------------------------------------------------------------------------
