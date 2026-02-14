*Algoritmo:*

Convierte un precio string a entero en ticks.
No usa decimales en operaciones matemáticas.
Los decimales se leen desde state/asset.

Ejemplo:
priceStr = "0.0150"
decimals(state) = 4
resultado = 150

-------------------------------------------------------------------------------
* Import getDecimals()                from   state/asset.rs


    function priceStringToTicks(priceStr)

        decimals = getDecimals()

        clean = reemplazar(priceStr, ".", "")

        parts = dividir(priceStr, ".")

        if length(parts) == 1:
            fracLen = 0
        else:
            fracLen = length(parts[1])

        if fracLen < decimals:
            zeros = repetir("0", decimals - fracLen)
            clean = concatenar(clean, zeros)

        if fracLen > decimals:
            clean = tomarPrimeros(length(clean) - (fracLen - decimals), clean)

        ticks = toInteger(clean)

        return ticks

-------------------------------------------------------------------------------
