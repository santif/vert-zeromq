vert-zeromq
===========

Providing a bridge from zero-mq to the vert-x event bus.

Create the bridge in your verticle start method.
```java
ZeroMQBridge bridge = new ZeroMQBridge("tcp://*:5558", vertx.eventBus());
bridge.start();

vertx.eventBus().registerHandler("testHandler", new Handler<Message<byte[]>>() {
        @Override
        public void handle(Message<byte[]> message) {
                message.reply(message.body());
        }
});
```

And then start a zeromq dealer on the client side.  Send the handler name as the first frame, and the message as the second.

```java
Socket client = ZMQ.context(1).socket(ZMQ.DEALER);
client.connect("http://localhost:5558");

client.send("testHandler",ZMQ.SNDMORE);
client.send("My message", 0);

```

