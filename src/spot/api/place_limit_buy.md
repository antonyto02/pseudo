*Algoritmo:*

Esta función crea una orden LIMIT BUY en Binance Spot.
Recibe price y qty.
Retorna ok + orderId + executedPrice para que el flujo pueda actualizar RAM.


-------------------------------------------------------------------------------
* Import getAsset()                    from   state/asset.rs


    function placeSpotLimitBuy(price, qty)

        asset = getAsset()

        baseUrl = getEnv("BINANCE_REST_BASE_URL")
        endpoint = "/v3/order"

        response = httpPostSigned(baseUrl + endpoint, {
            "symbol": asset,
            "side": "BUY",
            "type": "LIMIT",
            "timeInForce": "GTC",
            "quantity": qty,
            "price": price
        })

        if response.status != "ok":
            return { "ok": false }

        return {
            "ok": true,
            "orderId": response.orderId,
            "executedPrice": price
        }

-------------------------------------------------------------------------------
