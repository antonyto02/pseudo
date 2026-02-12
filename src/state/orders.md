# Orders structure initialization (easy pseudocode)

This file explains, step by step, how to build the in-memory structure for one trading pair.
The goal is to make clear **what each field means** before real implementation.

```pseudo
step 1) read pair global config

pair_config = {
  pair_id: "adausdt",

  # how many ticks between one buy order and the next buy order
  tick_spacing: 5,

  # maximum number of active buy orders allowed for this pair
  max_orders: 4,

  # when a buy is filled, sell price is created this many ticks above buy price
  sell_offset_ticks: 8,

  # token amount used in each single order
  tokens_per_order: 25
}


step 2) read current market prices

market = {
  bid_price: 0.1500,
  ask_price: 0.1510
}


step 3) initialize runtime state for this pair block

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


step 4) expected behavior after initialization

- buy_order_ids and sell_order_ids start empty
- filled_buy and filled_sell start in 0
- every new buy order should use config.tokens_per_order
- buy ladder distance should respect config.tick_spacing
- buy count must not exceed config.max_orders
- when one buy fills, create sell price using config.sell_offset_ticks above the buy fill price
```

## Example structure (json-like)

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
