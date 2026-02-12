El worker:

Espera eventos en una cola.

Procesa un evento a la vez.

Proceso del worker:

Espera un evento.

Si recibe PriceUpdate(new_best_bid):

Llama a analyze_price_update(new_best_bid) ubicado en price/price_analyzer.rs

