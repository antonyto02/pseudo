*Algoritmo:*

Esta función determina los precios donde deben estar las órdenes de compra.
Recibe bestBids como argumento en ticks enteros.
Toma el primer best bid como precio base para el bloque 1.
Luego, según `orderTickGap` y `blockCount`, arma la lista de n precios objetivo.


-------------------------------------------------------------------------------
* Import getAsset()                from   state/asset.rs
* Import getBlockCount()           from   state/asset.rs


    function findBuyLevels(bestBids)

        asset = getAsset()
        n = getBlockCount()
        gap = asset.orderTickGap

        basePrice = bestBids[0]

        buyLevels = []

        for i desde 0 hasta n - 1:
            price = basePrice - (i * gap)
            agregar price a buyLevels

        return buyLevels

-------------------------------------------------------------------------------
