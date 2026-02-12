# Inicialización de estructura de órdenes (pseudocódigo fácil)

Este archivo explica, paso a paso, cómo crear la estructura en memoria para un par.
La idea es entender **qué significa cada campo** antes de implementarlo en código real.

```pseudo
paso 1) leer la configuración global del par

pair_config = {
  pair_id: "adausdt",

  # cuántos ticks de distancia hay entre una orden de compra y la siguiente
  tick_spacing: 5,

  # número máximo de órdenes de compra activas para este par
  max_orders: 4,

  # cuando se llena una compra, la venta se calcula con estos ticks por encima
  sell_offset_ticks: 8,

  # cantidad de tokens usada en cada orden
  tokens_per_order: 25
}


paso 2) leer precios actuales del mercado

market = {
  bid_price: 0.1500,
  ask_price: 0.1510
}


paso 3) inicializar el estado de órdenes para este bloque

orders_state = {
  block_id: "b2",
  amount_target: 1000,

  config: pair_config,

  spot: {
    bid_price: market.bid_price,
    ask_price: market.ask_price,

    buy_order_ids: [],
    sell_order_ids: [],

    filled_buy: 0,
    filled_sell: 0
  }
}


paso 4) comportamiento esperado al iniciar

- buy_order_ids y sell_order_ids comienzan vacíos
- filled_buy y filled_sell comienzan en 0
- cada nueva compra debe usar config.tokens_per_order
- la separación entre compras debe respetar config.tick_spacing
- la cantidad de compras activas no debe superar config.max_orders
- al llenarse una compra, el precio de venta se calcula con config.sell_offset_ticks por encima del precio de llenado
```

## Ejemplo de estructura (json-like)

```json
{
  "block_id": "b2",
  "amount_target": 1000,
  "config": {
    "pair_id": "adausdt",
    "tick_spacing": 5,
    "max_orders": 4,
    "sell_offset_ticks": 8,
    "tokens_per_order": 25
  },
  "spot": {
    "bid_price": 0.1500,
    "ask_price": 0.1510,
    "buy_order_ids": [],
    "sell_order_ids": [],
    "filled_buy": 0,
    "filled_sell": 0
  }
}
```
