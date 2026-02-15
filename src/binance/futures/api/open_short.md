*Algoritmo:*

Esta función abre una posición short en Binance Futures con apalancamiento x1.
Recibe qty y retorna `ok` para que el analizador actualice el estado en RAM.


-------------------------------------------------------------------------------
* Import getAsset()                    from   state/asset.rs


    function openShort(qty)

        asset = getAsset()

        baseUrl = getEnv("BINANCE_FUTURES_REST_BASE_URL")
        endpoint = "/fapi/v1/order"

        response = httpPostSigned(baseUrl + endpoint, {
            "symbol": asset,
            "side": "SELL",
            "type": "MARKET",
            "quantity": qty
        })

        if response.status != "ok":
            return { "ok": false }

        return {
            "ok": true,
            "orderId": response.orderId
        }

-------------------------------------------------------------------------------
