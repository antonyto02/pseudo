Estructura del objeto en RAM:

Nota: en orders, todos los campos se manejan como string.
- bid_price: string o null
- ask_price: string o null
- buy_order_ids: lista de strings
- sell_order_ids: lista de strings
- filled_buy: string
- filled_sell: string
- has_open_short: string ("true" o "false")
- short_size: string

{
  "bid_price": null,
  "ask_price": null,
  "buy_order_ids": [],
  "sell_order_ids": [],
  "filled_buy": "0.0",
  "filled_sell": "0.0",
  "has_open_short": "false",
  "short_size": "0.0"
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
                "filled_buy": "0.0",
                "filled_sell": "0.0",
                "has_open_short": "false",
                "short_size": "0.0"
            }

            agregar block a blocks

        orderBlocks <- blocks


    function resetOrderBlock(index):
        orderBlocks[index] <- {
            "bid_price": null,
            "ask_price": null,
            "buy_order_ids": [],
            "sell_order_ids": [],
            "filled_buy": "0.0",
            "filled_sell": "0.0",
            "has_open_short": "false",
            "short_size": "0.0"
        }


    function setOrderBlock(index, blockData):
        orderBlocks[index] <- blockData


    function getOrderBlocks():
        return orderBlocks

-------------------------------------------------------
