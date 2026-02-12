# Inicialización de estructura de órdenes (pseudocódigo)

```pseudo
funcion inicializar_ordenes(block_id, amount_target, bid_price, ask_price):
    ordenes = {
        "block_id": block_id,
        "amount_target": amount_target,
        "spot": {
            "bid_price": bid_price,
            "ask_price": ask_price,
            "buy_order_ids": [],
            "sell_order_ids": [],
            "filled_buy": 0,
            "filled_sell": 0
        }
    }

    retornar ordenes
```

## Ejemplo de resultado

```json
{
  "block_id": "b2",
  "amount_target": 1000,
  "spot": {
    "bid_price": 0.1500,
    "ask_price": 0.1510,
    "buy_order_ids": ["123456"],
    "sell_order_ids": ["654321"],
    "filled_buy": 0,
    "filled_sell": 0
  }
}
```
