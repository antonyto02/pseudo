*Algoritmo:*

Convierte ticks enteros a precio string con decimales fijos.
No usa números decimales para cálculos.
Los decimales se leen desde state/asset.

Ejemplo:
ticks = 152
decimals(state) = 4
resultado = "0.0152"

-------------------------------------------------------------------------------
* Import getDecimals()                from   state/asset.rs


    function ticksToPriceString(ticks)

        decimals = getDecimals()

        raw = toString(ticks)

        minLen = decimals + 1
        if length(raw) < minLen:
            raw = padStart(raw, minLen, "0")

        splitPos = length(raw) - decimals

        intPart = substring(raw, 0, splitPos)
        fracPart = substring(raw, splitPos, length(raw))

        priceStr = concatenar(intPart, ".", fracPart)

        return priceStr

-------------------------------------------------------------------------------
