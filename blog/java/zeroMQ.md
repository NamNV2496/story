# ZeroMQ

dependence injection

```yaml
    <dependency>
      <groupId>org.zeromq</groupId>
      <artifactId>jeromq</artifactId>
      <version>0.4.3</version>
    </dependency>
```


init a content `Context context = ZMQ.context(1);`

# ========= publisher =========

new a socket `Socket nextNode = context.socket(ZMQ.PUB);`

bind to port send messages `client.bind("tcp://127.0.0.1:8888");`

send the name of topic first to receiver can read `client.sendMore(topic.getBytes());`

to send message `client.send("Test send from public side");`

# ========= forwarder =========

1. connect to socket to listen

Connect to port `frontend.connect("tcp://127.0.0.1:8888");`

Listen from frontend `frontend.subscribe("sendToForwarder".getBytes());`

2. bind to another port to forward

new a socket `Socket nextNode = context.socket(ZMQ.PUB);`

bind to port `frontend.connect("tcp://127.0.0.1:4444");`

send the name of topic first to receiver can read `nextNode.sendMore("sendToServer".getBytes());`

send messages `nextNode.send(message);`

# ========= subscriber =========

new a socket `Socket nextNode = context.socket(ZMQ.PUB);`

connect to socket `server.connect("tcp://127.0.0.1:4444");`

get message `while (!Thread.currentThread().isInterrupted())` parse `String payload = server.recvStr();` or `byte[] messageBytes = subscriber.recv();`


```java
        try (ZMQ.Context context = ZMQ.context(1);
            ZMQ.Socket publisher = context.socket(ZMQ.PUB)) {

            publisher.bind("tcp://*:5555");

            // Give some time for subscribers to connect
            Thread.sleep(1000);

            // Send a message to both subscribers
            String message = "Hello, subscribers!";
            publisher.send("".getBytes(), ZMQ.SNDMORE); // Empty frame for topic
            publisher.send(message.getBytes());

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
```


```java
        try (ZMQ.Context context = ZMQ.context(1);
            ZMQ.Socket subscriber = context.socket(ZMQ.SUB)) {

            subscriber.connect("tcp://localhost:5555");
            subscriber.subscribe("".getBytes());

            while (true) {
                // Receive and process the message from publisher
                byte[] topicBytes = subscriber.recv();
                byte[] messageBytes = subscriber.recv();
                String message = new String(messageBytes);
                System.out.println("Subscriber 1 received: " + message);
            }
        }
```