En este state se inicializa la variable `last_bid_price` como `null`.

---

Pseudocódigo (`market_state`):

    state MarketState:
        last_bid_price = null

    function getLastBidPrice():
        return last_bid_price

    function updateLastBidPrice(new_best_bid):
        # Validación básica
        if new_best_bid == null:
            return

        # Persistir el último best bid observado
        last_bid_price = new_best_bid
