Estructura del objeto en RAM:

{
    "block_id": "b2",
    "amount_target": 1000,
    "config": {
        "pair_id": "adausdt",
        "tick_spacing": 5,
        "max_orders": 4,
        "sell_offset_ticks": 8,
        "tokens_per_order": 25
    },
    "spot": {
        "bid_price": 0.1500,
        "ask_price": 0.1510,
        "buy_order_ids": [],
        "sell_order_ids": [],
        "filled_buy": 0,
        "filled_sell": 0
    }
}

-------------------------------------------------------
Métodos

    function getBlockId():
        return block_id

    function getAmountTarget():
        return amount_target

    function getConfig():
        return config

    function getSpotState():
        return spot

    function updateSpotPrices(new_bid_price, new_ask_price):
        if new_bid_price != null:
            spot.bid_price = new_bid_price
        if new_ask_price != null:
            spot.ask_price = new_ask_price

    function addBuyOrderId(order_id):
        if order_id == null:
            return
        if length(spot.buy_order_ids) >= config.max_orders:
            return
        append spot.buy_order_ids, order_id

    function addSellOrderId(order_id):
        if order_id == null:
            return
        append spot.sell_order_ids, order_id

    function incrementFilledBuy():
        spot.filled_buy = spot.filled_buy + 1

    function incrementFilledSell():
        spot.filled_sell = spot.filled_sell + 1
