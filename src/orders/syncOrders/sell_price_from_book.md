*Algoritmo:*

Esta función calcula el precio de venta usando ticks reales del orderbook.
No suma directo buyPrice + sellTickOffset.
Parte del nivel donde está buyPrice y avanza sellTickOffset posiciones en bestAsks.


-------------------------------------------------------------------------------

    function findSellPriceFromBook(buyPrice, sellTickOffset, bestAsks)

        startIndex = 0

        for i desde 0 hasta length(bestAsks) - 1:
            if bestAsks[i].price >= buyPrice:
                startIndex = i
                break

        targetIndex = startIndex + sellTickOffset

        if targetIndex >= length(bestAsks):
            targetIndex = length(bestAsks) - 1

        return bestAsks[targetIndex].price

-------------------------------------------------------------------------------
