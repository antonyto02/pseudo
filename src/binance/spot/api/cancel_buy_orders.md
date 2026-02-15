*Algoritmo:*

Esta función cancela en Binance Spot una lista de buy_order_ids.
Recibe los ids del bloque fuera de rango.
Si todas las cancelaciones responden ok, retorna true.
Si alguna falla, retorna false.


-------------------------------------------------------------------------------
* Import getAsset()                    from   state/asset.rs


    function cancelSpotBuyOrders(buyOrderIds)

        asset = getAsset()

        if length(buyOrderIds) == 0:
            return true

        baseUrl = getEnv("BINANCE_REST_BASE_URL")
        endpoint = "/v3/order"

        for each orderId in buyOrderIds:
            response = httpDeleteSigned(baseUrl + endpoint, {
                "symbol": asset,
                "orderId": orderId
            })

            if response.status != "ok":
                return false

        return true

-------------------------------------------------------------------------------
