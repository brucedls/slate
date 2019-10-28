# WebSocket API FAQs

## How to check the connection status?


IDAX solves this problem by heartbeat mechanism. The client-side can send a heartbeat data:{"event":"ping"}, and the server will respond to the client-side:{"event":"pong"} This shows that the connection is ok. If the client-side does not receive the heartbeat data from the server, the client-side needs to re-establish the connection.