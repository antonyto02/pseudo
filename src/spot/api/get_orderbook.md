*Algoritmo:*

Esta función obtiene el order book del par en Binance Spot.
Pide un snapshot con límite 10 para traer 10 best bids y 10 best asks.
Convierte los precios string de Binance a ticks enteros para uso interno en RAM.
Luego devuelve esos dos arreglos para que syncOrders los use.


-------------------------------------------------------------------------------
* Import getAsset()                from   state/asset.rs
* Import priceStringToTicks()      from   price/conversion/string_to_ticks.rs


    function getOrderBookSpot()

        asset = getAsset()

        baseUrl = getEnv("BINANCE_REST_BASE_URL")

        endpoint = "/v3/depth"

        response = httpGet(baseUrl + endpoint, {
            "symbol": asset,
            "limit": 10
        })

        bestBids = []
        bestAsks = []

        for each bidLevel in response.bids:
            bidPriceStr = bidLevel[0]
            bidTicks = priceStringToTicks(bidPriceStr)
            agregar bidTicks a bestBids

        for each askLevel in response.asks:
            askPriceStr = askLevel[0]
            askTicks = priceStringToTicks(askPriceStr)
            agregar askTicks a bestAsks

        return { "bestBids": bestBids, "bestAsks": bestAsks }

-------------------------------------------------------------------------------
