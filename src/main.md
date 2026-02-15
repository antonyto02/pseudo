Este archivo arranca el bot
1.- Crea las ordenes en RAM
2.- Sincroniza ordenes iniciales
3.- Abre el stream User Data
4.- Abre el stream BookTicker
5.- Arranca el worker de eventos

-------------------------------------------------------
Importaciones:

* Import createOrderBlocks()      from state/orders.rs
* Import syncOrders()             from orders/syncOrders/syncOrders.rs
* Import startUserDataStream()    from streams/user_data_stream.rs
* Import startBooktickerStream()  from streams/bookticker_stream.rs
* Import startEventWorker()       from worker/event_worker.rs
* Import createEventQueue()       from worker/event_worker.rs

-------------------------------------------------------

eventQueue = createEventQueue()

createOrderBlocks()
syncOrders()

startUserDataStream(eventQueue)
startBooktickerStream(eventQueue)
startEventWorker(eventQueue)
