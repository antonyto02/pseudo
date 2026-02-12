Importaciones:

* getLastBidPrice() se ubica en state/market_state.rs
* updateLastBidPrice() se ubica en state/market_state.rs
* handle_price_up() se ubica en price/price_up.rs
* handle_price_down() se ubica en price/price_down.rs

------------------------------------------------------

    analyze_price_update(new_best_bid)

        0.- last_bid_price = getLastBidPrice()
        1.- Si last_bid_price != null
                Si new_best_bid > last_bid_price, entonces handle_price_up()
                Si new_best_bid < last_bid_price, entonces handle_price_down()
        2.- updateLastBidPrice(new_best_bid)

------------------------------------------------------
