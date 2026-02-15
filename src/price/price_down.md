*Algoritmo:*

En este archivo se define la función handle_price_down(new_best_bid_ticks).
Cuando el precio baja, se ejecuta el analizador de apertura de short y luego
se llama a requeue().


-------------------------------------------------------------------------------
* Import shortOpenAnalyzer()   from   binance/futures/logic/short_open_analyzer.rs
* Import requeue()             from   orders/requeue/requeue.rs


    function handle_price_down(new_best_bid_ticks)

        shortOpenAnalyzer(new_best_bid_ticks)
        requeue()

-------------------------------------------------------------------------------
