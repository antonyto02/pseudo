*Algoritmo:*

Esta función asigna una compra al primer bloque vacío en RAM.
Busca el primer block con bid_price == null y guarda todos los campos.
Nota: en orders, bid_price, ask_price, ids, filleds y estado short se guardan como string.
No retorna nada.


-------------------------------------------------------------------------------
* Import getOrderBlocks()              from   state/orders.rs
* Import setOrderBlock()               from   state/orders.rs


    function assignBuyToFirstEmptyBlock(buyPrice, sellPrice, buyOrderId)

        orderBlocks = getOrderBlocks()

        for i desde 0 hasta length(orderBlocks) - 1:

            block = orderBlocks[i]

            if block.bid_price != null:
                continue

            newBlock = {
                "bid_price": toString(buyPrice),
                "ask_price": toString(sellPrice),
                "buy_order_ids": [toString(buyOrderId)],
                "sell_order_ids": [],
                "filled_buy": "0.0",
                "filled_sell": "0.0",
                "has_open_short": "false",
                "short_size": "0.0"
            }

            setOrderBlock(i, newBlock)
            break

-------------------------------------------------------------------------------
