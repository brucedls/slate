# WebSocket API常见问题

## 如何判断连接是否断开？

IDAX通过心跳机制解决这个问题。客户端可以发送一次心跳数据：{"event":"ping"}，服务器会响应客户端：{"event":"pong"} 以此来表明客户端和服务端保持正常连接。如果客户端未接到服务端响应的心跳数据则需要客户端重新建立连接。