Estructura del objeto en RAM:

Nota: `last_bid_price` se guarda en ticks (entero), no en string de precio.
Ejemplo: si llega "0.0171", en RAM se guarda `171` (con decimals=4).

{
    "last_bid_price":null
}

-------------------------------------------------------
Métodos


    function getLastBidPrice():
        return last_bid_price

    function updateLastBidPrice(new_best_bid_ticks):
        if new_best_bid_ticks == null:
            return
        last_bid_price = new_best_bid_ticks
        
-------------------------------------------------------
