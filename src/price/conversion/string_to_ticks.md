*Algoritmo:*

Convierte un precio string a entero en ticks.
No usa decimales en operaciones matemáticas.

Ejemplo:
priceStr = "0.0150"
decimals = 4
resultado = 150

-------------------------------------------------------------------------------

    function priceStringToTicks(priceStr, decimals)

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
