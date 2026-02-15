*Algoritmo:*

En este archivo se define la función handle_price_up().
Cuando el precio sube, se ejecuta shortExitAnalyzer() y luego syncOrders().


-------------------------------------------------------------------------------
* Import shortExitAnalyzer()   from   binance/futures/logic/short_exit_analyzer.rs
* Import syncOrders()          from   orders/syncOrders/syncOrders.rs


    function handle_price_up()

        shortExitAnalyzer()
        syncOrders()

-------------------------------------------------------------------------------
