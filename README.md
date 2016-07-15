# Akka Schema Evolution with Shopping Carts and Event Adapters #
This example is intended to demonstrate how to perform Schema Evolution without using an encoding mechanism that 
supports schema evolution (e.g. Protocol Buffers, Avro and Thrift).

In the following example, we use BooPickle as our encoding mechanism which only performs encoding and does not support
schema evolution (we are treating the encoding like it doesn't). We rely on Event Adapters to promote from lower 
versions to higher versions which essentially does the Schema Evolution.

Here is what the set up looks like when persisting:
Shopping Cart Actor -> Persist Message V2 -> Event Adapter (pass through) -> Serializer -> Journal
Note: the event adapter is not being used for V2 events since its a pass-through

Here is the setup for recovering:
V1:
Shopping Cart Actor <- Recover Message V2 <- Event Adapter (promote V1 ~> V2) <- Serializer (V1) <- Journal

V2:
Shopping Cart Actor <- Recover Message V2 <- Event Adapter (pass through) <- Serializer (V2) <- Journal
Note: the event adapter is not being used for V2 events since its a pass-through

# How to use the application #
Revert to f6058d3 to persist V1 events
Revert to master to persist V2 events and support the promotion from V1 ~> V2