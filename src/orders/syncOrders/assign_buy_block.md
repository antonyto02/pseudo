*Algoritmo:*

Esta función asigna una compra al primer bloque vacío en RAM.
Busca el primer block con bid_price == null y guarda todos los campos.
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
                "bid_price": buyPrice,
                "ask_price": sellPrice,
                "buy_order_ids": [buyOrderId],
                "sell_order_ids": [],
                "filled_buy": 0.0,
                "filled_sell": 0.0
            }

            setOrderBlock(i, newBlock)
            break

-------------------------------------------------------------------------------
