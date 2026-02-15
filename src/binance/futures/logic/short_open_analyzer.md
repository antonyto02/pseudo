*Algoritmo:*

En este archivo se define la función shortOpenAnalyzer(new_best_bid).
La función recorre los bloques de órdenes en RAM y abre short cuando:
1) el bloque no tiene short abierto (`has_open_short == "false"`), y
2) `new_best_bid` es menor al `bid_price` del bloque.

Si se cumple, calcula la cantidad a abrir en x1 como:
`qtyPerBlock - filled_sell`.
Luego llama a openShort(qty). Si Binance responde ok, marca el bloque con short abierto y guarda el tamaño en `short_size`.


-------------------------------------------------------------------------------
* Import getOrderBlocks()   from   state/orders.rs
* Import setOrderBlock()    from   state/orders.rs
* Import getAsset()         from   state/asset.rs
* Import openShort()        from   binance/futures/api/open_short.rs


    function shortOpenAnalyzer(new_best_bid)

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

            if new_best_bid >= block.bid_price:
                continue

            shortQty = asset.qtyPerBlock - block.filled_sell

            if shortQty <= 0:
                continue

            result = openShort(shortQty)

            if result.ok == false:
                continue

            block.has_open_short = "true"
            block.short_size = toString(shortQty)

            setOrderBlock(i, block)

-------------------------------------------------------------------------------
