Estructura del objeto en RAM:

{
  "bid_price": null,
  "ask_price": null,
  "buy_order_ids": [],
  "sell_order_ids": [],
  "filled_buy": 0.0,
  "filled_sell": 0.0
}



-------------------------------------------------------
Métodos

    function createOrderBlocks():
        n <- getBlockCount()
        blocks <- []

        for i desde 0 hasta n - 1:
            block <- {
                "bid_price": null,
                "ask_price": null,
                "buy_order_ids": [],
                "sell_order_ids": [],
                "filled_buy": 0.0,
                "filled_sell": 0.0
            }

            agregar block a blocks

        orderBlocks <- blocks

-------------------------------------------------------
