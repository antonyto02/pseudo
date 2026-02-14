*Algoritmo:*

Esta función obtiene el order book del par en Binance Spot.
Pide un snapshot con límite 10 para traer 10 best bids y 10 best asks.
Luego devuelve esos dos arreglos para que syncOrders los use.


-------------------------------------------------------------------------------
* Import getAsset()                from   state/asset.rs


    function getOrderBookSpot()

        asset = getAsset()

        baseUrl = getEnv("BINANCE_REST_BASE_URL")

        endpoint = "/v3/depth"

        response = httpGet(baseUrl + endpoint, {
            "symbol": asset,
            "limit": 10
        })

        bestBids = response.bids
        bestAsks = response.asks

        return { "bestBids": bestBids, "bestAsks": bestAsks }

-------------------------------------------------------------------------------
