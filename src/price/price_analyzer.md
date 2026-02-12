Importaciones:

* last_bid_price se ubica de state/market_state.rs
* handle_price_up() se ubica en price/price_up.rs
* handle_price_down() se ubica en price/price_down.rs

------------------------------------------------------

    analyze_price_update(new_best_bid)

        1.- Si lastBidPrice = !null
                Si new_best_bid > last_bid_price, entonces handle_price_up()
                Si new_best_bid < last_bid_price, entonces handle_price_down()
        2.- updateLastBidPrice()

------------------------------------------------------

