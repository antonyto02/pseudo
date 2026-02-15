*Algoritmo:*

En este archivo se define la función llamada analyzePriceUpdate() la cual recibe como parámetro una variable llamada new_best_bid. El propósito de esta función es analizar si el best bid subió o bajó; seguido de ello, llamar a la función correspondiente (handle_price_up() o handle_price_down()). En caso de mantener el mismo bid price, no se dispara niguna logica adicional. Finalmente, independientemnte del análisis del precio, se actualiza en memoria el last_bid_price con el nuevo que entró.


-------------------------------------------------------------------------------
* Import getLastBidPrice()     from   state/market_state.rs
* Import updateLastBidPrice()  from   state/market_state.rs
* Import handle_price_up()     from   price/price_up.rs
* Import handle_price_down()   from   price/price_down.rs



    function analyzePriceUpdate(new_best_bid)


        last_bid_price = getLastBidPrice()

        Si last_bid_price != null 
                Si new_best_bid > last_bid_price, entonces handle_price_up()
                Si new_best_bid < last_bid_price, entonces handle_price_down(new_best_bid)

        updateLastBidPrice(new_best_bid)

-------------------------------------------------------------------------------


