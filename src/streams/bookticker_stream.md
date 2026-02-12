Este archivo contiene funciones:

Se suscribe al WebSocket bookticker de la criptomoneda que esta en asset.rs.

Escucha eventos continuamente.

Filtra y limpia la información recibida de manera que extrae únicamente el best_bid.

Crea un evento PriceUpdate(new_best_bid).

Envía ese evento al worker mediante la cola.