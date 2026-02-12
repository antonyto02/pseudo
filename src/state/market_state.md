Estructura del objeto en RAM:

{
    "last_bid_price":null
}

-------------------------------------------------------
Métodos


    function getLastBidPrice():
        return last_bid_price

    function updateLastBidPrice(new_best_bid):
        if new_best_bid == null:
            return
        last_bid_price = new_best_bid
-------------------------------------------------------
